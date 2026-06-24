# CopyObject

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Creates a copy of an object that is already stored in the storage system.

Objects up to 5 GB in size can be copied in a single atomic operation. To copy objects larger than 5 GB, the multipart upload [UploadPartCopy](UploadPartCopy.md) API must be used instead.

**Permissions:** The client credentials must be authorized to perform the operation on the target bucket. Under the standard S3 IAM policy model, this requires the `s3:GetObject` permission on the source object and the `s3:PutObject` permission on the destination bucket.

**Concurrency:** Concurrent write requests to the same key are resolved using Last-Writer-Wins (LWW) semantics. The request that completes last will overwrite any other concurrent writes. From the client's perspective, the final ordering of overlapping requests is undefined.

**Conditional Copying:** Clients can perform conditional copying using headers like `x-amz-copy-source-if-match`, `x-amz-copy-source-if-none-match`, `x-amz-copy-source-if-modified-since`, or `x-amz-copy-source-if-unmodified-since`. For example, setting `x-amz-copy-source-if-match` ensures the copy succeeds only if the source object's ETag matches the specified value.

**Encryption:** When copying an object, the destination object can be encrypted using server-side encryption. The target encryption configuration is determined by the default configuration of the destination bucket, unless overridden by specifying encryption headers like `x-amz-server-side-encryption` or customer-managed keys.

**Metadata and Tagging:** By default, object metadata and tags are copied from the source object. This behavior can be controlled using the `x-amz-metadata-directive` and `x-amz-tagging-directive` headers. If set to `REPLACE`, new metadata or tags can be supplied in the request.

**Response and Error Handling:** For HTTP 1.1 requests, the response may be chunk-encoded. If an error occurs after the copy starts, the error response might be embedded within a `200 OK` status code. Clients must parse the entire XML response body to verify if the copy operation was completed successfully.

## Request

```HTTP
PUT /{bucket}/{key} HTTP/1.1
x-amz-copy-source: {CopySource}
```

### Path: `bucket`
The name of the destination bucket.

### Path: `key`
The key of the destination object.

### Header: `x-amz-acl`
> **Avoid:** Canned ACLs are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

The canned Access Control List (ACL) to apply to the object copy.

### Header: `cache-control`
Specifies caching behavior along the request/reply chain.

### Header: `x-amz-checksum-algorithm`
Specifies the algorithm used to compute the checksum for the object payload (e.g., `CRC32`, `CRC32C`, `SHA1`, `SHA256`).

### Header: `content-disposition`
Specifies presentational information for the object.

### Header: `content-encoding`
Specifies content encodings applied to the object, indicating what decoding mechanisms must be applied to obtain the media-type referenced by the Content-Type header.

### Header: `content-language`
The language the content is in.

### Header: `content-type`
A standard MIME type describing the format of the object payload.

### Header: `x-amz-copy-source`
Specifies the source object for the copy operation.

This header specifies the name of the source bucket and the key of the source object, separated by a slash (e.g., `source-bucket/source-key`). The value must be URL-encoded.

If source bucket versioning is enabled, this header identifies the current version of the object to copy. To copy a specific version, append the version ID as a query parameter (e.g., `source-bucket/source-key?versionId=version-id`).

### Header: `x-amz-copy-source-if-match`
Copies the object if its entity tag (ETag) matches the specified ETag value.

### Header: `x-amz-copy-source-if-modified-since`
Copies the object if it has been modified since the specified time.

### Header: `x-amz-copy-source-if-none-match`
Copies the object if its entity tag (ETag) is different from the specified ETag value.

### Header: `x-amz-copy-source-if-unmodified-since`
Copies the object if it hasn't been modified since the specified time.

### Header: `expires`
The date and time at which the object is no longer cacheable.

### Header: `x-amz-grant-full-control`
> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants full control permissions to specified grantees on the object copy.

### Header: `x-amz-grant-read`
> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants read permissions to specified grantees on the object copy.

### Header: `x-amz-grant-read-acp`
> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants read-ACP permissions to specified grantees on the object copy.

### Header: `x-amz-grant-write-acp`
> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

Grants write-ACP permissions to specified grantees on the object copy.

### Header: `if-match`
Copies the object only if the destination object's ETag matches the specified ETag. This supports conditional operations.

### Header: `if-none-match`
Copies the object only if the destination key does not already exist. When set to `*`, the copy fails if the destination object already exists.

### Header: `x-amz-meta-*`
A prefix used to specify custom user-defined metadata key-value pairs stored with the object copy.

Each metadata item is sent as a dynamic header (e.g., `x-amz-meta-key-name: value`). This is a key S3 protocol pattern. Note that standard OpenAPI schemas do not support dynamic wildcard header definitions, requiring custom handling or dictionary serialization in client implementations.

### Header: `x-amz-metadata-directive`
Specifies whether the metadata is copied from the source object (`COPY`) or replaced with the metadata provided in the request (`REPLACE`). If not specified, the default behavior is `COPY`.

### Header: `x-amz-tagging-directive`
Specifies whether the object tag-set is copied from the source object (`COPY`) or replaced with the tag-set provided in the request (`REPLACE`). If not specified, the default behavior is `COPY`.

### Header: `x-amz-object-annotation-directive`
> **Note:** Likely not portable. May be removed.

Specifies whether to copy annotations from the source object (`COPY`) or exclude them (`EXCLUDE`). If not specified, the default behavior is `COPY`.

### Header: `x-amz-server-side-encryption`
Specifies the server-side encryption algorithm to use when storing the object. Common values include `AES256` (SSE-S3) and `aws:kms` (SSE-KMS).

### Header: `x-amz-storage-class`
Specifies the storage class to use for storing the object copy. If not specified, the storage engine uses its default storage class.

### Header: `x-amz-website-redirect-location`
Specifies the redirect location for requests to this object when the bucket is configured as a static website. The redirect target can be a relative path or an external URL.

### Header: `x-amz-server-side-encryption-customer-algorithm`
Specifies the server-side encryption algorithm to use for encrypting the object copy with customer-provided keys (e.g., `AES256`).

### Header: `x-amz-server-side-encryption-customer-key`
Specifies the customer-provided encryption key. The key must match the algorithm specified in the algorithm header.

### Header: `x-amz-server-side-encryption-customer-key-md5`
Provides the MD5 digest of the customer-provided encryption key for integrity validation.

### Header: `x-amz-server-side-encryption-aws-kms-key-id`
Specifies the KMS key identifier to use for encrypting the object copy.

### Header: `x-amz-server-side-encryption-context`
Specifies a base64-encoded UTF-8 string containing key-value pairs used as additional encryption context for KMS encryption.

### Header: `x-amz-server-side-encryption-bucket-key-enabled`
Specifies whether to use an S3 Bucket Key for server-side encryption with KMS keys (SSE-KMS).

### Header: `x-amz-copy-source-server-side-encryption-customer-algorithm`
Specifies the server-side encryption algorithm used to encrypt the source object (e.g., `AES256`). Required if the source object was encrypted with SSE-C.

### Header: `x-amz-copy-source-server-side-encryption-customer-key`
Specifies the customer-provided encryption key used to encrypt the source object. Required if the source object was encrypted with SSE-C.

### Header: `x-amz-copy-source-server-side-encryption-customer-key-md5`
Provides the MD5 digest of the customer-provided encryption key used to encrypt the source object for integrity validation.

### Header: `x-amz-request-payer`
Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-tagging`
A set of tags to associate with the object copy, formatted as URL-encoded key-value pairs. This is used when `x-amz-tagging-directive` is set to `REPLACE`.

### Header: `x-amz-object-lock-mode`
Specifies the Object Lock retention mode (`RETENTION` or `COMPLIANCE`) to apply to the object copy.

### Header: `x-amz-object-lock-retain-until-date`
Specifies the timestamp when the Object Lock retention period should expire on the object copy.

### Header: `x-amz-object-lock-legal-hold`
Specifies whether to apply a legal hold (`ON` or `OFF`) to the object copy.

### Header: `x-amz-expected-bucket-owner`
The account identifier of the expected destination bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-source-expected-bucket-owner`
The account identifier of the expected source bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<CopyObjectResult>
	<ETag>string</ETag>
	<LastModified>timestamp</LastModified>
	<ChecksumType>string</ChecksumType>
	<ChecksumCRC32>string</ChecksumCRC32>
	<ChecksumCRC32C>string</ChecksumCRC32C>
	<ChecksumCRC64NVME>string</ChecksumCRC64NVME>
	<ChecksumSHA1>string</ChecksumSHA1>
	<ChecksumSHA256>string</ChecksumSHA256>
	<ChecksumSHA512>string</ChecksumSHA512>
	<ChecksumMD5>string</ChecksumMD5>
	<ChecksumXXHASH64>string</ChecksumXXHASH64>
	<ChecksumXXHASH3>string</ChecksumXXHASH3>
	<ChecksumXXHASH128>string</ChecksumXXHASH128>
</CopyObjectResult>
```

### Response Header: `x-amz-expiration`
If the destination object's expiration is configured by a lifecycle rule, this header indicates when the object copy becomes eligible for deletion.

### Response Header: `x-amz-copy-source-version-id`
Indicates the version ID of the source object that was copied.

### Response Header: `x-amz-version-id`
Indicates the version ID of the newly created object copy in the destination bucket.

### Response Header: `x-amz-server-side-encryption`
Specifies the server-side encryption algorithm used to store the object copy.

### Response Header: `x-amz-server-side-encryption-customer-algorithm`
Indicates the server-side encryption algorithm used if the object copy was encrypted with customer-provided keys.

### Response Header: `x-amz-server-side-encryption-customer-key-md5`
Indicates the MD5 digest of the customer-provided key used if the object copy was encrypted with customer-provided keys.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`
Indicates the KMS key identifier used if the object copy was encrypted with SSE-KMS.

### Response Header: `x-amz-server-side-encryption-context`
Indicates the KMS encryption context used if the object copy was encrypted with SSE-KMS.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`
Indicates whether an S3 Bucket Key was used for server-side encryption with KMS keys (SSE-KMS).

### Response Header: `x-amz-request-charged`
Indicates that the requester was charged for processing the request.

### Body: `<CopyObjectResult>`
Container for all response elements.
Type: [CopyObjectResult](shared-shapes.md#schema-copyobjectresult)

## Errors

### `403` `ObjectNotInActiveTierError`
The source object of the COPY action is not in the active tier and is only stored in Amazon S3 Glacier.

```HTTP
HTTP/1.1 403 Forbidden
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>ObjectNotInActiveTierError</Code>
	<Message>The source object of the COPY action is not in the active tier and is only stored in Amazon S3 Glacier.</Message>
</Error>
```

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#CopyObject": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#CopyObjectRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#CopyObjectOutput"
      },
      "errors": [
        {
          "target": "com.amazonaws.s3#ObjectNotInActiveTierError"
        }
      ],
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#copyobject"
        },
        "smithy.api#examples": [
          {
            "title": "To copy an object",
            "documentation": "The following example copies an object from one bucket to another.",
            "input": {
              "Bucket": "destinationbucket",
              "CopySource": "/sourcebucket/HappyFacejpg",
              "Key": "HappyFaceCopyjpg"
            },
            "output": {
              "CopyObjectResult": {
                "LastModified": "2016-12-15T17:38:53.000Z",
                "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\""
              }
            }
          }
        ],
        "smithy.api#http": {
          "method": "PUT",
          "uri": "/{Bucket}/{Key+}?x-id=CopyObject",
          "code": 200
        },
        "smithy.rules#staticContextParams": {
          "DisableS3ExpressSessionAuth": {
            "value": true
          }
        }
      }
    },
    "com.amazonaws.s3#CopyObjectOutput": {
      "type": "structure",
      "members": {
        "CopyObjectResult": {
          "target": "com.amazonaws.s3#CopyObjectResult",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#body-copyobjectresult"
            },
            "smithy.api#httpPayload": {}
          }
        },
        "Expiration": {
          "target": "com.amazonaws.s3#Expiration",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-expiration"
            },
            "smithy.api#httpHeader": "x-amz-expiration"
          }
        },
        "CopySourceVersionId": {
          "target": "com.amazonaws.s3#CopySourceVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-copy-source-version-id"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-version-id"
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
        "ServerSideEncryption": {
          "target": "com.amazonaws.s3#ServerSideEncryption",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption"
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
    "com.amazonaws.s3#CopyObjectRequest": {
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
        "ChecksumAlgorithm": {
          "target": "com.amazonaws.s3#ChecksumAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-checksum-algorithm"
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
        "ContentType": {
          "target": "com.amazonaws.s3#ContentType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-content-type"
            },
            "smithy.api#httpHeader": "Content-Type"
          }
        },
        "CopySource": {
          "target": "com.amazonaws.s3#CopySource",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source"
            },
            "smithy.api#httpHeader": "x-amz-copy-source",
            "smithy.api#required": {},
            "smithy.rules#contextParam": {
              "name": "CopySource"
            }
          }
        },
        "CopySourceIfMatch": {
          "target": "com.amazonaws.s3#CopySourceIfMatch",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-if-match"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-if-match"
          }
        },
        "CopySourceIfModifiedSince": {
          "target": "com.amazonaws.s3#CopySourceIfModifiedSince",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-if-modified-since"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-if-modified-since"
          }
        },
        "CopySourceIfNoneMatch": {
          "target": "com.amazonaws.s3#CopySourceIfNoneMatch",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-if-none-match"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-if-none-match"
          }
        },
        "CopySourceIfUnmodifiedSince": {
          "target": "com.amazonaws.s3#CopySourceIfUnmodifiedSince",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-if-unmodified-since"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-if-unmodified-since"
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
        "Metadata": {
          "target": "com.amazonaws.s3#Metadata",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-meta-*"
            },
            "smithy.api#httpPrefixHeaders": "x-amz-meta-"
          }
        },
        "MetadataDirective": {
          "target": "com.amazonaws.s3#MetadataDirective",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-metadata-directive"
            },
            "smithy.api#httpHeader": "x-amz-metadata-directive"
          }
        },
        "TaggingDirective": {
          "target": "com.amazonaws.s3#TaggingDirective",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-tagging-directive"
            },
            "smithy.api#httpHeader": "x-amz-tagging-directive"
          }
        },
        "AnnotationDirective": {
          "target": "com.amazonaws.s3#AnnotationDirective",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-object-annotation-directive"
            },
            "smithy.api#httpHeader": "x-amz-object-annotation-directive"
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
        "CopySourceSSECustomerAlgorithm": {
          "target": "com.amazonaws.s3#CopySourceSSECustomerAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-server-side-encryption-customer-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-server-side-encryption-customer-algorithm"
          }
        },
        "CopySourceSSECustomerKey": {
          "target": "com.amazonaws.s3#CopySourceSSECustomerKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-server-side-encryption-customer-key"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-server-side-encryption-customer-key"
          }
        },
        "CopySourceSSECustomerKeyMD5": {
          "target": "com.amazonaws.s3#CopySourceSSECustomerKeyMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-server-side-encryption-customer-key-md5"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-server-side-encryption-customer-key-MD5"
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
        },
        "ExpectedSourceBucketOwner": {
          "target": "com.amazonaws.s3#AccountId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-source-expected-bucket-owner"
            },
            "smithy.api#httpHeader": "x-amz-source-expected-bucket-owner"
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
