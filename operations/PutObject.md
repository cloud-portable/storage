# PutObject

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Adds an object to a bucket. If an object with the same key already exists, the storage engine overwrites the existing object.

**Concurrency:** Concurrent write requests to the same key are resolved using Last-Writer-Wins (LWW) semantics. The request that completes last will overwrite any other concurrent writes. From the client's perspective, the final ordering of overlapping requests is undefined.

**Conditional Writes:** Clients can perform conditional uploads using headers like `If-None-Match` or `If-Match`. For example, setting `If-None-Match: *` ensures the upload succeeds only if the object key does not already exist, returning a `412 Precondition Failed` error on conflict.

**Versioning:** If bucket versioning is enabled, overwriting an existing object creates a new object version rather than replacing the existing data. The previous version remains accessible, and the write operation returns a unique version identifier (`x-amz-version-id`).

**Data Integrity:** To ensure data is not corrupted during transit, clients can specify the `Content-MD5` header or use SDK-managed checksum headers (e.g., `x-amz-sdk-checksum-algorithm` alongside `x-amz-checksum-sha256`). The storage engine validates the uploaded payload against the provided hash and rejects the request if they do not match.

**Permissions:** The client credentials must be authorized to perform the operation on the target bucket. Under the standard S3 IAM policy model, this requires the `s3:PutObject` action. Additional permissions like `s3:PutObjectAcl` or `s3:PutObjectTagging` are required if the request specifies canned ACLs or tags, respectively.

## Request

```HTTP
PUT /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`

The name of the bucket associated with the upload.

### Path: `key`

The object key for which the PUT action was initiated.

### Header: `x-amz-acl`

> **Avoid:** Canned ACLs are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

The canned Access Control List (ACL) to apply to the object.

### Header: `cache-control`

Specifies caching behavior along the request/reply chain.

### Header: `content-disposition`

Specifies presentational information for the object.

### Header: `content-encoding`

Specifies content encodings applied to the object, indicating what decoding mechanisms must be applied to obtain the media-type referenced by the Content-Type header.

### Header: `content-language`

The language the content is in.

### Header: `content-length`

Size of the body in bytes. This is useful when the size of the request body cannot be determined automatically.

### Header: `content-md5`

The Base64 encoded 128-bit MD5 digest of the object payload for integrity validation.

### Header: `content-type`

A standard MIME type describing the format of the object payload.

### Header: `x-amz-sdk-checksum-algorithm`

Specifies the algorithm used to compute the checksum for the object payload (e.g., `CRC32`, `CRC32C`, `SHA1`, `SHA256`).

When this header is specified, the client must also provide the matching checksum digest header, such as `x-amz-checksum-sha256`. The storage engine validates the payload against the provided digest and returns a `BadDigest` error if they do not match.

### Header: `x-amz-checksum-crc32`

The Base64 encoded 32-bit CRC32 checksum of the object.

### Header: `x-amz-checksum-crc32c`

The Base64 encoded 32-bit CRC32C checksum of the object.

### Header: `x-amz-checksum-crc64nvme`

The Base64 encoded 64-bit CRC64NVME checksum of the object.

### Header: `x-amz-checksum-sha1`

The Base64 encoded 160-bit SHA1 digest of the object.

### Header: `x-amz-checksum-sha256`

The Base64 encoded 256-bit SHA256 digest of the object.

### Header: `x-amz-checksum-sha512`

The Base64 encoded 512-bit SHA512 digest of the object.

### Header: `x-amz-checksum-md5`

The Base64 encoded 128-bit MD5 checksum of the object.

### Header: `x-amz-checksum-xxhash64`

The Base64 encoded 64-bit XXHASH64 checksum of the object.

### Header: `x-amz-checksum-xxhash3`

The Base64 encoded 64-bit XXHASH3 checksum of the object.

### Header: `x-amz-checksum-xxhash128`

The Base64 encoded 128-bit XXHASH128 checksum of the object.

### Header: `expires`

The date and time at which the object is no longer cacheable.

### Header: `if-match`

Uploads the object only if its current ETag matches the specified ETag. If they do not match, the request fails with a `412 Precondition Failed` error.

### Header: `if-none-match`

Uploads the object only if the key does not already exist in the bucket. When set to `*`, the operation fails with a `412 Precondition Failed` error if the object exists.

### Header: `x-amz-grant-full-control`

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants full control permissions to specified grantees.

### Header: `x-amz-grant-read`

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants read permissions to specified grantees.

### Header: `x-amz-grant-read-acp`

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants read-ACP permissions to specified grantees.

### Header: `x-amz-grant-write-acp`

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants write-ACP permissions to specified grantees.

### Header: `x-amz-write-offset-bytes`

> **Note:** Likely not portable. May be removed.

Specifies the offset for appending data to existing objects in bytes. The offset must be equal to the current size of the target object.

### Header: `x-amz-meta-*`

A prefix used to specify custom user-defined metadata key-value pairs stored with the object.

Each metadata item is sent as a dynamic header (e.g., `x-amz-meta-key-name: value`). This is a key S3 protocol pattern. Note that standard OpenAPI schemas do not support dynamic wildcard header definitions, requiring custom handling or dictionary serialization in client implementations.

### Header: `x-amz-server-side-encryption`

Specifies the server-side encryption algorithm to use when storing the object. Common values include `AES256` (SSE-S3) and `aws:kms` (SSE-KMS).

### Header: `x-amz-storage-class`

Specifies the storage class to use for storing the object. If not specified, the storage engine uses its default storage class.

### Header: `x-amz-website-redirect-location`

Specifies the redirect location for requests to this object when the bucket is configured as a static website. The redirect target can be a relative path or an external URL.

### Header: `x-amz-server-side-encryption-customer-algorithm`

Specifies the server-side encryption algorithm to use for encrypting the object with customer-provided keys (e.g., `AES256`).

### Header: `x-amz-server-side-encryption-customer-key`

Specifies the customer-provided encryption key. The key must match the algorithm specified in the algorithm header.

### Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation.

### Header: `x-amz-server-side-encryption-aws-kms-key-id`

Specifies the KMS key identifier to use for object encryption. If not specified, the default AWS managed key is typically used if SSE-KMS encryption is requested.

### Header: `x-amz-server-side-encryption-context`

Specifies a base64-encoded UTF-8 string containing key-value pairs used as additional encryption context for KMS encryption.

### Header: `x-amz-server-side-encryption-bucket-key-enabled`

Specifies whether to use an S3 Bucket Key for server-side encryption with KMS keys (SSE-KMS).

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-tagging`

A set of tags to associate with the object, formatted as URL-encoded key-value pairs (e.g., `Key1=Value1&Key2=Value2`).

### Header: `x-amz-object-lock-mode`

Specifies the Object Lock retention mode (`RETENTION` or `COMPLIANCE`) to apply to the object.

### Header: `x-amz-object-lock-retain-until-date`

Specifies the timestamp when the Object Lock retention period should expire.

### Header: `x-amz-object-lock-legal-hold`

Specifies whether to apply a legal hold (`ON` or `OFF`) to the object.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Body: `blob`

The object payload data to be stored.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
```

### Response Header: `x-amz-expiration`

If an object expiration lifecycle rule matches the uploaded object, this header indicates when the object becomes eligible for deletion.

### Response Header: `etag`

The entity tag (ETag) representing the stored object.

For single-part uploads, the ETag is the double-quoted MD5 hash of the object payload. For multipart uploads, it is typically the MD5 hash of the concatenated MD5 digests of each uploaded part, followed by a hyphen and the total part count (e.g., `"<hash>-N"`). If the object is encrypted on the server-side, the ETag may not represent the MD5 hash of the payload.

### Response Header: `x-amz-checksum-crc32`

The Base64 encoded 32-bit CRC32 checksum of the object.

### Response Header: `x-amz-checksum-crc32c`

The Base64 encoded 32-bit CRC32C checksum of the object.

### Response Header: `x-amz-checksum-crc64nvme`

The Base64 encoded 64-bit CRC64NVME checksum of the object.

### Response Header: `x-amz-checksum-sha1`

The Base64 encoded 160-bit SHA1 digest of the object.

### Response Header: `x-amz-checksum-sha256`

The Base64 encoded 256-bit SHA256 digest of the object.

### Response Header: `x-amz-checksum-sha512`

The Base64 encoded 512-bit SHA512 digest of the object.

### Response Header: `x-amz-checksum-md5`

The Base64 encoded 128-bit MD5 checksum of the object.

### Response Header: `x-amz-checksum-xxhash64`

The Base64 encoded 64-bit XXHASH64 checksum of the object.

### Response Header: `x-amz-checksum-xxhash3`

The Base64 encoded 64-bit XXHASH3 checksum of the object.

### Response Header: `x-amz-checksum-xxhash128`

The Base64 encoded 128-bit XXHASH128 checksum of the object.

### Response Header: `x-amz-checksum-type`

Specifies the checksum type of the object, which determines how part-level checksums are combined.

### Response Header: `x-amz-server-side-encryption`

The server-side encryption algorithm used to protect the object (e.g., `AES256` or `aws:kms`).

### Response Header: `x-amz-version-id`

The version identifier of the stored object, returned if versioning is enabled on the bucket.

### Response Header: `x-amz-server-side-encryption-customer-algorithm`

Confirms the server-side encryption algorithm used when customer-provided encryption keys (SSE-C) were requested.

### Response Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation when SSE-C was used.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`

The KMS key identifier used for encrypting the object, if SSE-KMS was requested.

### Response Header: `x-amz-server-side-encryption-context`

The base64-encoded KMS encryption context associated with the object.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`

Indicates whether an S3 Bucket Key was used for SSE-KMS encryption.

### Response Header: `x-amz-object-size`

> **Note:** Likely not portable. May be removed.

The size of the object in bytes.

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

## Errors

### `400` `EncryptionTypeMismatch`
The existing object was created with a different encryption type. Subsequent write requests must include the appropriate encryption parameters in the request or while creating the session.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>EncryptionTypeMismatch</Code>
	<Message>The existing object was created with a different encryption type. Subsequent write requests must include the appropriate encryption parameters in the request or while creating the session.</Message>
</Error>
```

### `400` `InvalidRequest`
A parameter or header in your request isn't valid. For details, see the description of this API operation.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>InvalidRequest</Code>
	<Message>A parameter or header in your request isn't valid. For details, see the description of this API operation.</Message>
</Error>
```

### `400` `InvalidWriteOffset`
The write offset value that you specified does not match the current object size.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>InvalidWriteOffset</Code>
	<Message>The write offset value that you specified does not match the current object size.</Message>
</Error>
```

### `400` `TooManyParts`
You have attempted to add more parts than the maximum of 10000 that are allowed for this object. You can use the CopyObject operation to copy this object to another and then add more data to the newly copied object.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>TooManyParts</Code>
	<Message>You have attempted to add more parts than the maximum of 10000 that are allowed for this object. You can use the CopyObject operation to copy this object to another and then add more data to the newly copied object.</Message>
</Error>
```

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#PutObject": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#PutObjectRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#PutObjectOutput"
      },
      "errors": [
        {
          "target": "com.amazonaws.s3#EncryptionTypeMismatch"
        },
        {
          "target": "com.amazonaws.s3#InvalidRequest"
        },
        {
          "target": "com.amazonaws.s3#InvalidWriteOffset"
        },
        {
          "target": "com.amazonaws.s3#TooManyParts"
        }
      ],
      "traits": {
        "aws.protocols#httpChecksum": {
          "requestAlgorithmMember": "ChecksumAlgorithm"
        },
        "smithy.api#documentation": {
          "$ref": "#putobject"
        },
        "smithy.api#examples": [
          {
            "title": "To create an object.",
            "documentation": "The following example creates an object. If the bucket is versioning enabled, S3 returns version ID in response.",
            "input": {
              "Body": "filetoupload",
              "Bucket": "examplebucket",
              "Key": "objectkey"
            },
            "output": {
              "VersionId": "Bvq0EDKxOcXLJXNo_Lkz37eM3R4pfzyQ",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\""
            }
          },
          {
            "title": "To upload an object (specify optional headers)",
            "documentation": "The following example uploads an object. The request specifies optional request headers to directs S3 to use specific storage class and use server-side encryption.",
            "input": {
              "Body": "HappyFace.jpg",
              "Bucket": "examplebucket",
              "Key": "HappyFace.jpg",
              "ServerSideEncryption": "AES256",
              "StorageClass": "STANDARD_IA"
            },
            "output": {
              "VersionId": "CG612hodqujkf8FaaNfp8U..FIhLROcp",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\"",
              "ServerSideEncryption": "AES256"
            }
          },
          {
            "title": "To upload an object",
            "documentation": "The following example uploads an object to a versioning-enabled bucket. The source file is specified using Windows file syntax. S3 returns VersionId of the newly created object.",
            "input": {
              "Body": "HappyFace.jpg",
              "Bucket": "examplebucket",
              "Key": "HappyFace.jpg"
            },
            "output": {
              "VersionId": "tpf3zF08nBplQK1XLOefGskR7mGDwcDk",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\""
            }
          },
          {
            "title": "To upload an object and specify canned ACL.",
            "documentation": "The following example uploads and object. The request specifies optional canned ACL (access control list) to all READ access to authenticated users. If the bucket is versioning enabled, S3 returns version ID in response.",
            "input": {
              "ACL": "authenticated-read",
              "Body": "filetoupload",
              "Bucket": "examplebucket",
              "Key": "exampleobject"
            },
            "output": {
              "VersionId": "Kirh.unyZwjQ69YxcQLA8z4F5j3kJJKr",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\""
            }
          },
          {
            "title": "To upload an object and specify optional tags",
            "documentation": "The following example uploads an object. The request specifies optional object tags. The bucket is versioned, therefore S3 returns version ID of the newly created object.",
            "input": {
              "Body": "c:\\HappyFace.jpg",
              "Bucket": "examplebucket",
              "Key": "HappyFace.jpg",
              "Tagging": "key1=value1&key2=value2"
            },
            "output": {
              "VersionId": "psM2sYY4.o1501dSx8wMvnkOzSBB.V4a",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\""
            }
          },
          {
            "title": "To upload an object and specify server-side encryption and object tags",
            "documentation": "The following example uploads an object. The request specifies the optional server-side encryption option. The request also specifies optional object tags. If the bucket is versioning enabled, S3 returns version ID in response.",
            "input": {
              "Body": "filetoupload",
              "Bucket": "examplebucket",
              "Key": "exampleobject",
              "ServerSideEncryption": "AES256",
              "Tagging": "key1=value1&key2=value2"
            },
            "output": {
              "VersionId": "Ri.vC6qVlA4dEnjgRV4ZHsHoFIjqEMNt",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\"",
              "ServerSideEncryption": "AES256"
            }
          },
          {
            "title": "To upload object and specify user-defined metadata",
            "documentation": "The following example creates an object. The request also specifies optional metadata. If the bucket is versioning enabled, S3 returns version ID in response.",
            "input": {
              "Body": "filetoupload",
              "Bucket": "examplebucket",
              "Key": "exampleobject",
              "Metadata": {
                "metadata1": "value1",
                "metadata2": "value2"
              }
            },
            "output": {
              "VersionId": "pSKidl4pHBiNwukdbcPXAIs.sshFFOc0",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\""
            }
          }
        ],
        "smithy.api#http": {
          "method": "PUT",
          "uri": "/{Bucket}/{Key+}?x-id=PutObject",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#PutObjectOutput": {
      "type": "structure",
      "members": {
        "Expiration": {
          "target": "com.amazonaws.s3#Expiration",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-expiration"
            },
            "smithy.api#httpHeader": "x-amz-expiration"
          }
        },
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-etag"
            },
            "smithy.api#httpHeader": "ETag"
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-crc32"
            },
            "smithy.api#httpHeader": "x-amz-checksum-crc32"
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-crc32c"
            },
            "smithy.api#httpHeader": "x-amz-checksum-crc32c"
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-crc64nvme"
            },
            "smithy.api#httpHeader": "x-amz-checksum-crc64nvme"
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-sha1"
            },
            "smithy.api#httpHeader": "x-amz-checksum-sha1"
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-sha256"
            },
            "smithy.api#httpHeader": "x-amz-checksum-sha256"
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-sha512"
            },
            "smithy.api#httpHeader": "x-amz-checksum-sha512"
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-md5"
            },
            "smithy.api#httpHeader": "x-amz-checksum-md5"
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-xxhash64"
            },
            "smithy.api#httpHeader": "x-amz-checksum-xxhash64"
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-xxhash3"
            },
            "smithy.api#httpHeader": "x-amz-checksum-xxhash3"
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-xxhash128"
            },
            "smithy.api#httpHeader": "x-amz-checksum-xxhash128"
          }
        },
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-type"
            },
            "smithy.api#httpHeader": "x-amz-checksum-type"
          }
        },
        "ServerSideEncryption": {
          "target": "com.amazonaws.s3#ServerSideEncryption",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption"
          }
        },
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-version-id"
            },
            "smithy.api#httpHeader": "x-amz-version-id"
          }
        },
        "SSECustomerAlgorithm": {
          "target": "com.amazonaws.s3#SSECustomerAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption-customer-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-customer-algorithm"
          }
        },
        "SSECustomerKeyMD5": {
          "target": "com.amazonaws.s3#SSECustomerKeyMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption-customer-key-md5"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-customer-key-MD5"
          }
        },
        "SSEKMSKeyId": {
          "target": "com.amazonaws.s3#SSEKMSKeyId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption-aws-kms-key-id"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-aws-kms-key-id"
          }
        },
        "SSEKMSEncryptionContext": {
          "target": "com.amazonaws.s3#SSEKMSEncryptionContext",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption-context"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-context"
          }
        },
        "BucketKeyEnabled": {
          "target": "com.amazonaws.s3#BucketKeyEnabled",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption-bucket-key-enabled"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-bucket-key-enabled"
          }
        },
        "Size": {
          "target": "com.amazonaws.s3#Size",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-object-size"
            },
            "smithy.api#httpHeader": "x-amz-object-size"
          }
        },
        "RequestCharged": {
          "target": "com.amazonaws.s3#RequestCharged",
          "traits": {
            "smithy.api#httpHeader": "x-amz-request-charged",
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-request-charged"
            }
          }
        }
      },
      "traits": {
        "smithy.api#output": {}
      }
    },
    "com.amazonaws.s3#PutObjectRequest": {
      "type": "structure",
      "members": {
        "ACL": {
          "target": "com.amazonaws.s3#ObjectCannedACL",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-acl"
            },
            "smithy.api#httpHeader": "x-amz-acl"
          }
        },
        "Body": {
          "target": "com.amazonaws.s3#StreamingBlob",
          "traits": {
            "smithy.api#default": "",
            "smithy.api#documentation": {
              "$ref": "#body-blob"
            },
            "smithy.api#httpPayload": {}
          }
        },
        "Bucket": {
          "target": "com.amazonaws.s3#BucketName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#path-bucket"
            },
            "smithy.api#httpLabel": {},
            "smithy.api#required": {},
            "smithy.rules#contextParam": {
              "name": "Bucket"
            }
          }
        },
        "CacheControl": {
          "target": "com.amazonaws.s3#CacheControl",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-cache-control"
            },
            "smithy.api#httpHeader": "Cache-Control"
          }
        },
        "ContentDisposition": {
          "target": "com.amazonaws.s3#ContentDisposition",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-disposition"
            },
            "smithy.api#httpHeader": "Content-Disposition"
          }
        },
        "ContentEncoding": {
          "target": "com.amazonaws.s3#ContentEncoding",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-encoding"
            },
            "smithy.api#httpHeader": "Content-Encoding"
          }
        },
        "ContentLanguage": {
          "target": "com.amazonaws.s3#ContentLanguage",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-language"
            },
            "smithy.api#httpHeader": "Content-Language"
          }
        },
        "ContentLength": {
          "target": "com.amazonaws.s3#ContentLength",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-length"
            },
            "smithy.api#httpHeader": "Content-Length"
          }
        },
        "ContentMD5": {
          "target": "com.amazonaws.s3#ContentMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-md5"
            },
            "smithy.api#httpHeader": "Content-MD5"
          }
        },
        "ContentType": {
          "target": "com.amazonaws.s3#ContentType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-type"
            },
            "smithy.api#httpHeader": "Content-Type"
          }
        },
        "ChecksumAlgorithm": {
          "target": "com.amazonaws.s3#ChecksumAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-sdk-checksum-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-sdk-checksum-algorithm"
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-crc32"
            },
            "smithy.api#httpHeader": "x-amz-checksum-crc32"
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-crc32c"
            },
            "smithy.api#httpHeader": "x-amz-checksum-crc32c"
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-crc64nvme"
            },
            "smithy.api#httpHeader": "x-amz-checksum-crc64nvme"
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-sha1"
            },
            "smithy.api#httpHeader": "x-amz-checksum-sha1"
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-sha256"
            },
            "smithy.api#httpHeader": "x-amz-checksum-sha256"
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-sha512"
            },
            "smithy.api#httpHeader": "x-amz-checksum-sha512"
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-md5"
            },
            "smithy.api#httpHeader": "x-amz-checksum-md5"
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-xxhash64"
            },
            "smithy.api#httpHeader": "x-amz-checksum-xxhash64"
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-xxhash3"
            },
            "smithy.api#httpHeader": "x-amz-checksum-xxhash3"
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-xxhash128"
            },
            "smithy.api#httpHeader": "x-amz-checksum-xxhash128"
          }
        },
        "Expires": {
          "target": "com.amazonaws.s3#Expires",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-expires"
            },
            "smithy.api#httpHeader": "Expires"
          }
        },
        "IfMatch": {
          "target": "com.amazonaws.s3#IfMatch",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-if-match"
            },
            "smithy.api#httpHeader": "If-Match"
          }
        },
        "IfNoneMatch": {
          "target": "com.amazonaws.s3#IfNoneMatch",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-if-none-match"
            },
            "smithy.api#httpHeader": "If-None-Match"
          }
        },
        "GrantFullControl": {
          "target": "com.amazonaws.s3#GrantFullControl",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-grant-full-control"
            },
            "smithy.api#httpHeader": "x-amz-grant-full-control"
          }
        },
        "GrantRead": {
          "target": "com.amazonaws.s3#GrantRead",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-grant-read"
            },
            "smithy.api#httpHeader": "x-amz-grant-read"
          }
        },
        "GrantReadACP": {
          "target": "com.amazonaws.s3#GrantReadACP",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-grant-read-acp"
            },
            "smithy.api#httpHeader": "x-amz-grant-read-acp"
          }
        },
        "GrantWriteACP": {
          "target": "com.amazonaws.s3#GrantWriteACP",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-grant-write-acp"
            },
            "smithy.api#httpHeader": "x-amz-grant-write-acp"
          }
        },
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#path-key"
            },
            "smithy.api#httpLabel": {},
            "smithy.api#required": {},
            "smithy.rules#contextParam": {
              "name": "Key"
            }
          }
        },
        "WriteOffsetBytes": {
          "target": "com.amazonaws.s3#WriteOffsetBytes",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-write-offset-bytes"
            },
            "smithy.api#httpHeader": "x-amz-write-offset-bytes"
          }
        },
        "Metadata": {
          "target": "com.amazonaws.s3#Metadata",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-meta-*"
            },
            "smithy.api#httpPrefixHeaders": "x-amz-meta-"
          }
        },
        "ServerSideEncryption": {
          "target": "com.amazonaws.s3#ServerSideEncryption",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption"
          }
        },
        "StorageClass": {
          "target": "com.amazonaws.s3#StorageClass",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-storage-class"
            },
            "smithy.api#httpHeader": "x-amz-storage-class"
          }
        },
        "WebsiteRedirectLocation": {
          "target": "com.amazonaws.s3#WebsiteRedirectLocation",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-website-redirect-location"
            },
            "smithy.api#httpHeader": "x-amz-website-redirect-location"
          }
        },
        "SSECustomerAlgorithm": {
          "target": "com.amazonaws.s3#SSECustomerAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption-customer-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-customer-algorithm"
          }
        },
        "SSECustomerKey": {
          "target": "com.amazonaws.s3#SSECustomerKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption-customer-key"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-customer-key"
          }
        },
        "SSECustomerKeyMD5": {
          "target": "com.amazonaws.s3#SSECustomerKeyMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption-customer-key-md5"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-customer-key-MD5"
          }
        },
        "SSEKMSKeyId": {
          "target": "com.amazonaws.s3#SSEKMSKeyId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption-aws-kms-key-id"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-aws-kms-key-id"
          }
        },
        "SSEKMSEncryptionContext": {
          "target": "com.amazonaws.s3#SSEKMSEncryptionContext",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption-context"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-context"
          }
        },
        "BucketKeyEnabled": {
          "target": "com.amazonaws.s3#BucketKeyEnabled",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-server-side-encryption-bucket-key-enabled"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-bucket-key-enabled"
          }
        },
        "RequestPayer": {
          "target": "com.amazonaws.s3#RequestPayer",
          "traits": {
            "smithy.api#httpHeader": "x-amz-request-payer",
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-request-payer"
            }
          }
        },
        "Tagging": {
          "target": "com.amazonaws.s3#TaggingHeader",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-tagging"
            },
            "smithy.api#httpHeader": "x-amz-tagging"
          }
        },
        "ObjectLockMode": {
          "target": "com.amazonaws.s3#ObjectLockMode",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-object-lock-mode"
            },
            "smithy.api#httpHeader": "x-amz-object-lock-mode"
          }
        },
        "ObjectLockRetainUntilDate": {
          "target": "com.amazonaws.s3#ObjectLockRetainUntilDate",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-object-lock-retain-until-date"
            },
            "smithy.api#httpHeader": "x-amz-object-lock-retain-until-date"
          }
        },
        "ObjectLockLegalHoldStatus": {
          "target": "com.amazonaws.s3#ObjectLockLegalHoldStatus",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-object-lock-legal-hold"
            },
            "smithy.api#httpHeader": "x-amz-object-lock-legal-hold"
          }
        },
        "ExpectedBucketOwner": {
          "target": "com.amazonaws.s3#AccountId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-expected-bucket-owner"
            },
            "smithy.api#httpHeader": "x-amz-expected-bucket-owner"
          }
        }
      },
      "traits": {
        "smithy.api#input": {}
      }
    }
  }
}
```

</details>
