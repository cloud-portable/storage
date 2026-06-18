# OpenAPI compatibiltiy

S3 does things that cannot be expressed in openAPI 3.1, and are unlikely to ever be supported.

| Issue | Can Smithy represent it? | Can OpenAPI?| Mitigation |
| :--- | :--- | :--- | :--- |
| **HTTP Path Overload** | **Yes** (via separate Operation shapes with `@http` traits) | **No** (Path keys must be unique) | Merge into single endpoint with combined doc prose. |
| **Slashes in Key** | **Yes** (via `/{Key+}` syntax) | **No** (Assumes single segment) | Document that parameter allows slashes; disable client URL-encoding. |
| **`x-amz-meta-*` Headers** | **Yes** (via `@httpPrefixHeaders` trait) | **No** (Requires static header keys) | Document the prefix behavior in the parameter description. |
| **SigV4 signing** | **Yes** (via `@aws.auth#sigv4` trait) | **No** (Only generic header inputs) | Require manual SDK signing middleware plugins. |
| **XML Namespaces** | **Yes** (via `@xmlNamespace` trait) | **Partial** (Supports basic namespaces, but loose validation) | Rely on manual XML serializer overrides. |


## HTTP Path Overloading

S3 overloads `PUT /{Bucket}/{Key}` to serve two completely different operations:
  *   `PutObject` (uploads a file)
  *   `CopyObject` (copies an existing file, triggered by the presence of the `x-amz-copy-source` header)

OpenAPI is strictly path-and-method centric. A path key (like `/Bucket/Key`) and method (like `PUT`) can only map to **one** operation definition, **one** request body schema, and **one** set of responses.

OpenAPI cannot represent conditional request dispatching based on the presence or absence of a header (`x-amz-copy-source`). We are forced to merge them into a single endpoint containing both schemas under a flexible union type (e.g. `oneOf` request payloads/headers), losing the fine-grained, machine-readable separation of the two APIs.

## Greedy Path Parameters (slashes in keys)

S3 object keys can contain forward slashes (e.g., `/my-bucket/photos/2026/summer.jpg`). In S3, the `{Key}` parameter is a "greedy" match that consumes the entire remaining URI path, slashes included.

OpenAPI path templates assume standard REST patterns where path parameters represent a single, slash-delimited URI segment.

There is no way in OpenAPI to specify that a parameter is "greedy" or allowed to contain raw `/` characters. If you pass this spec to standard SDK client generators, they will URL-encode the slashes (converting `/` to `%2F`), which S3 routing gateways reject, or they will fail to compile the routing table.


## Wildcard Metadata Headers

When calling `GetObject` or `HeadObject`, S3 returns user-defined metadata as HTTP response headers prefixed with `x-amz-meta-` (e.g., `x-amz-meta-author: Alice`, `x-amz-meta-project: Cloud`).

OpenAPI requires that all response headers in a `Headers Object` be defined using **static, explicit names**.

OpenAPI has no mechanism to define a wildcard or pattern-matching header constraint (like `x-amz-meta-*`). You cannot represent a dynamic dictionary mapped to response headers. In the schema, this metadata must be omitted or described as a single generic description, meaning clients cannot automatically generate typed models for custom metadata.


## SigV4 signing

S3 requires AWS Signature Version 4 (SigV4) for authentication. This requires clients to calculate an HMAC-SHA256 signature of a canonicalized request string (including headers, query parameters, and body) and send it in the `Authorization` header.

OpenAPI's `securitySchemes` support standard protocols (OAuth2, HTTP Basic/Bearer, API Key). It cannot express cryptographic requirements or computation logic for headers.

To an OpenAPI client generator, the security parameters look like standard static headers (`Authorization`, `x-amz-content-sha256`, `x-amz-date`). A generated client will simply expect the developer to pass static string values for these headers. The generator cannot generate the cryptographic signing middleware required to actually make a successful request.


## XML Schemas

S3 payloads (like `ListObjectsV2` and `DeleteObjects`) are XML documents. Many of these elements require specific XML namespaces (e.g. `xmlns="http://s3.amazonaws.com/doc/2006-03-01/"`) on the root tag.

While OpenAPI has a basic `xml` object (supporting `name`, `namespace`, `prefix`, and `attribute`), it lacks full XML Schema Definition (XSD) capabilities.
t cannot represent constraints such as:
  *   Strict element ordering (sequence vs choice).
  *   Enforcing that a specific namespace must be declared on the root tag but *not* on child elements.
  *   XML processing instructions.
  Consequently, OpenAPI validation tools cannot strictly validate S3 XML payloads to the same depth that an XSD schema can.


## HTTP Error Overloading
If an S3 operation fails, it returns a `404 Not Found` or `403 Forbidden` with a structured XML body indicating the exact error code (e.g. `<Code>NoSuchKey</Code>` or `<Code>NoSuchBucket</Code>`).

While OpenAPI 3.1 supports mapping multiple schemas to a single status code using `oneOf`, it lacks a machine-readable way to bind specific runtime header values or XML text content to determine which schema applies.

Client SDKs cannot automatically map the generic `404` error payload to specific, typed exceptions (like throwing a `NoSuchKeyException` vs a `NoSuchBucketException`) based purely on the OpenAPI spec constraints. They must parse the XML body manually in custom error-handling code.

