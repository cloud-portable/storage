# Smithy Tooling Evaluation: S3 API & OpenAPI Compilation

This document summarizes our research, findings, and technical evaluation of the AWS Smithy tooling ecosystem when compiling a vendor-neutral, portable S3 specification subset.

## The Smithy Tooling Ecosystem

### What Works
* `smithy-cli` from homebrew works and is self-contained, and bundles its own private Java Runtime Environment (JRE).
* `smithy validate` and `smithy format` work well.
* Generating a AST JSON file `smithy ast` works, and gives us something we can work with.

### What Doesn't
*   **Native HTML Documentation Generation**: The official documentation generator plugin (`smithy-docgen`) is currently in an early pre-release state (version `0.1.0`). It is not published to Maven Central, requiring engineers to clone the repository and publish it to `mavenLocal` via Gradle. The HTML doc pipeline is not yet out-of-the-box ready for pure CLI usage.


## AWS S3 Extensions in Smithy

The canonical models rely on AWS-specific traits. These are not part of the core Smithy language specification and must be resolved as external Maven dependencies (`software.amazon.smithy:smithy-aws-traits`):

1.  **Auth & Protocols**: `@aws.auth#sigv4`, `@aws.api#service`, and `@aws.protocols#restXml`.
2.  **Endpoint Routing**: `@smithy.rules#endpointRuleSet`, `@smithy.rules#endpointBdd`, and `@smithy.rules#clientContextParams` (which encode AWS-specific regional partitioning, FIPS, and DualStack endpoints).
3.  **Integrations & Verification**: `@smithy.rules#endpointTests` (incorporating tests for operations like `WriteGetObjectResponse` or Outposts).
4.  **Checksums**: `@aws.protocols#httpChecksum` (for specifying SHA/CRC validations).


## Exporting Smithy to OpenAPI

The official Smithy-to-OpenAPI build plugin (`software.amazon.smithy:smithy-openapi`) has issues:

### HTTP Path Overloading
S3 routes multiple distinct operations to the same HTTP path and method. For example:
*   `PUT /{Bucket}/{Key}` -> Uploads a new object (`PutObject`)
*   `PUT /{Bucket}/{Key}` -> Copies an existing object (`CopyObject`)

The official Smithy OpenAPI generator resolves this by appending query discriminator strings to the path keys inside the OpenAPI JSON:
*   `"/{Bucket}/{Key+}?x-id=PutObject"`
*   `"/{Bucket}/{Key+}?x-id=CopyObject"`
    
> [!NOTE]
> The `x-id` parameter is solely a validation workaround in the Smithy model; it does not exist on the wire in S3 requests, and actual S3 servers do not expect it (routing is done using headers like `x-amz-copy-source` instead). In our generated operation documentation, we special-case and strip `x-id` from all HTTP request codeblocks to ensure they accurately represent wire-level S3 traffic.

This violates the [OpenAPI Specification (Paths Object)](https://spec.openapis.org/oas/v3.1.0#paths-object), which states: *"Query parameters and fragment identifiers MUST NOT be included in the path."*

This invalid path syntax breaks third-party OpenAPI tooling.

### XML responses
S3 is an XML-based service communicating via the `@aws.protocols#restXml` protocol.

The official Smithy OpenAPI build plugin only supports JSON protocols (like `aws.protocols#restJson1` and `smithy.protocols#rpcv2`) out-of-the-box. It lacks a protocol serializer for `restXml`, causing the build to fail.

### Unsupported Schema Traits
Traits like `@httpPrefixHeaders` (used by S3 for custom user metadata `x-amz-meta-*`) and `@httpChecksum` have no direct equivalents in OpenAPI. By default, the compiler aborts. To compile, the configuration must explicitly ignore these:
```json
"plugins": {
  "openapi": {
    "onHttpPrefixHeaders": "WARN",
    "ignoreUnsupportedTraits": true
  }
}
```

## Smithy references

- https://smithy.io/2.0/aws/customizations/s3-customizations.html
- https://smithy.io/2.0/aws/protocols/aws-restxml-protocol.html

