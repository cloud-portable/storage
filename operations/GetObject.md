# GetObject

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Retrieves an object from the storage system.

In the request, specify the bucket name and the key of the object to retrieve.

**Permissions:** The client credentials must be authorized to perform this operation. Under the standard S3 IAM policy model, this requires the `s3:GetObject` permission on the object. If retrieving a specific version of the object, the client requires `s3:GetObjectVersion` permission.

**Overriding Response Headers:** Clients can dynamically override certain metadata headers in the response (such as `Content-Type`, `Cache-Control`, or `Content-Disposition`) by specifying the corresponding `response-*` query parameters. These overrides are only returned on successful `200 OK` responses and are useful for customizing client download behaviors.

**Range Retrieval:** The operation supports retrieving specific byte ranges of an object using the standard `Range` HTTP header, or specific part indices using the `partNumber` query parameter.

**Data Integrity:** Checksums can be retrieved and validated by specifying the `x-amz-checksum-mode` header.

## Request

```HTTP
GET /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`
The name of the bucket containing the object.

### Path: `key`
The key of the object to retrieve.

### Query: `response-cache-control`
Sets the `Cache-Control` header of the response.

### Query: `response-content-disposition`
Sets the `Content-Disposition` header of the response.

### Query: `response-content-encoding`
Sets the `Content-Encoding` header of the response.

### Query: `response-content-language`
Sets the `Content-Language` header of the response.

### Query: `response-content-type`
Sets the `Content-Type` header of the response.

### Query: `response-expires`
Sets the `Expires` header of the response.

### Query: `versionId`
Retrieves a specific version of the object instead of the latest version. If the specified version is a delete marker, the response returns a `405 Method Not Allowed` error.

### Query: `partNumber`
Retrieves a specific part of a multipart object. This is a positive integer between 1 and 10,000. Effectively performs a ranged GET request for the specified part.

### Header: `if-match`
Return the object only if its entity tag (ETag) matches the specified value; otherwise, return a `412 Precondition Failed` error.

### Header: `if-modified-since`
Return the object only if it has been modified since the specified time; otherwise, return a `304 Not Modified` status code.

### Header: `if-none-match`
Return the object only if its entity tag (ETag) is different from the specified value; otherwise, return a `304 Not Modified` status code.

### Header: `if-unmodified-since`
Return the object only if it hasn't been modified since the specified time; otherwise, return a `412 Precondition Failed` error.

### Header: `range`
Downloads the specified byte range of an object. The range format must comply with standard HTTP specifications (e.g., `bytes=0-1048575`). Note that most S3-compatible storage engines do not support retrieving multiple ranges in a single request.

### Header: `x-amz-server-side-encryption-customer-algorithm`
Specifies the encryption algorithm used to decrypt the object (e.g., `AES256`). Required if the object was encrypted with customer-provided keys (SSE-C).

### Header: `x-amz-server-side-encryption-customer-key`
Specifies the customer-provided encryption key used to decrypt the object. Required if the object was encrypted with customer-provided keys (SSE-C).

### Header: `x-amz-server-side-encryption-customer-key-md5`
Provides the MD5 digest of the customer-provided encryption key for integrity validation. Required if the object was encrypted with customer-provided keys (SSE-C).

### Header: `x-amz-request-payer`
Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`
The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-checksum-mode`
Enables retrieving the object checksums in the response by setting this header to `ENABLED`. When enabled, the response will include the corresponding checksum headers if they were calculated during upload.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/octet-stream
```

### Response Header: `x-amz-delete-marker`
Indicates whether the retrieved object is a delete marker. If set to `true`, the object has been soft-deleted.

### Response Header: `accept-ranges`
Indicates whether the server supports range requests for objects. Usually returns `bytes`.

### Response Header: `x-amz-expiration`
If the object is subject to an expiration lifecycle rule, this header indicates when the object becomes eligible for deletion.

### Response Header: `x-amz-restore`
If the object was restored from an archive storage class, this header provides information about the restoration status and when the restored copy expires.

### Response Header: `last-modified`
The date and time when the object was last modified.

### Response Header: `content-length`
The size of the returned object body in bytes. If a range is requested, this represents the size of the range.

### Response Header: `etag`
The entity tag (ETag) representing the stored object. For single-part uploads, the ETag is the double-quoted MD5 hash of the object payload. For multipart uploads, it is typically the MD5 hash of the concatenated MD5 digests of each uploaded part, followed by a hyphen and the total part count (e.g., `"<hash>-N"`). If the object is encrypted on the server-side, the ETag may not represent the MD5 hash of the payload.

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
The Base64 encoded 128-bit MD5 digest of the object.

### Response Header: `x-amz-checksum-xxhash64`
The Base64 encoded 64-bit XXHASH64 checksum of the object.

### Response Header: `x-amz-checksum-xxhash3`
The Base64 encoded 64-bit XXHASH3 checksum of the object.

### Response Header: `x-amz-checksum-xxhash128`
The Base64 encoded 128-bit XXHASH128 checksum of the object.

### Response Header: `x-amz-checksum-type`
Indicates how the object's checksum was calculated (e.g., combining part-level checksums for multipart uploads).

### Response Header: `x-amz-missing-meta`
Indicates the number of custom metadata headers that were not returned due to formatting limitations.

### Response Header: `x-amz-version-id`
The version identifier of the retrieved object.

### Response Header: `cache-control`
Specifies caching behavior along the request/reply chain.

### Response Header: `content-disposition`
Specifies presentational information for the object.

### Response Header: `content-encoding`
Specifies content encodings applied to the object, indicating what decoding mechanisms must be applied to obtain the media-type referenced by the Content-Type header.

### Response Header: `content-language`
The language the content is in.

### Response Header: `content-range`
Indicates the range of bytes returned by this response (e.g., `bytes 0-1048575/5242880` where the last number is the total object size).

### Response Header: `content-type`
A standard MIME type describing the format of the object data.

### Response Header: `expires`
The date and time at which the object is no longer cacheable.

### Response Header: `x-amz-website-redirect-location`
Specifies the redirect location for requests to this object when the bucket is configured as a static website.

### Response Header: `x-amz-server-side-encryption`
Specifies the server-side encryption algorithm used when storing the object.

### Response Header: `x-amz-meta-*`
A prefix used to specify custom user-defined metadata key-value pairs stored with the object. Each metadata item is returned as a dynamic header (e.g., `x-amz-meta-key-name: value`).

### Response Header: `x-amz-server-side-encryption-customer-algorithm`
Indicates the server-side encryption algorithm used if the object was encrypted with customer-provided keys.

### Response Header: `x-amz-server-side-encryption-customer-key-md5`
Provides the MD5 digest of the customer-provided encryption key if the object was encrypted with customer-provided keys.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`
Indicates the KMS key identifier used if the object was encrypted with SSE-KMS.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`
Indicates whether the object uses an S3 Bucket Key for server-side encryption with KMS keys (SSE-KMS).

### Response Header: `x-amz-storage-class`
Provides storage class information of the object.

### Response Header: `x-amz-request-charged`
Indicates that the requester was charged for processing the request.

### Response Header: `x-amz-replication-status`
Indicates the status of replication for this object (e.g., `PENDING`, `COMPLETED`, `FAILED`).

### Response Header: `x-amz-mp-parts-count`
The count of parts this object has. Only returned if the object was uploaded as a multipart upload and a specific `partNumber` was requested.

### Response Header: `x-amz-tagging-count`
The number of tags associated with this object.

### Response Header: `x-amz-object-lock-mode`
Specifies the Object Lock retention mode (`RETENTION` or `COMPLIANCE`) currently in place for this object.

### Response Header: `x-amz-object-lock-retain-until-date`
Specifies the timestamp when the Object Lock retention period is set to expire on this object.

### Response Header: `x-amz-object-lock-legal-hold`
Specifies whether a legal hold is applied to this object (`ON` or `OFF`).

### Body: `blob`
Object data.

## Errors

### `403` `InvalidObjectState`
The object is archived and must be restored before it can be retrieved.

```HTTP
HTTP/1.1 403 Forbidden
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>InvalidObjectState</Code>
	<Message>Object is archived and inaccessible until restored.</Message>
</Error>
```

### `404` `NoSuchKey`
The specified key does not exist.

```HTTP
HTTP/1.1 404 Not Found
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>NoSuchKey</Code>
	<Message>The specified key does not exist.</Message>
</Error>
```

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#GetObject": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#GetObjectRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#GetObjectOutput"
      },
      "errors": [
        {
          "target": "com.amazonaws.s3#InvalidObjectState"
        },
        {
          "target": "com.amazonaws.s3#NoSuchKey"
        }
      ],
      "traits": {
        "aws.protocols#httpChecksum": {
          "requestValidationModeMember": "ChecksumMode",
          "responseAlgorithms": [
            "CRC64NVME",
            "CRC32",
            "CRC32C",
            "SHA256",
            "SHA1",
            "SHA512",
            "MD5",
            "XXHASH64",
            "XXHASH3",
            "XXHASH128"
          ]
        },
        "smithy.api#documentation": {
          "$ref": "#getobject"
        },
        "smithy.api#examples": [
          {
            "title": "To retrieve a byte range of an object ",
            "documentation": "The following example retrieves an object for an S3 bucket. The request specifies the range header to retrieve a specific byte range.",
            "input": {
              "Bucket": "examplebucket",
              "Key": "SampleFile.txt",
              "Range": "bytes=0-9"
            },
            "output": {
              "AcceptRanges": "bytes",
              "ContentType": "text/plain",
              "LastModified": "2014-10-09T22:57:28.000Z",
              "ContentLength": 10,
              "VersionId": "null",
              "ETag": "\"0d94420ffd0bc68cd3d152506b97a9cc\"",
              "ContentRange": "bytes 0-9/43",
              "Metadata": {}
            }
          },
          {
            "title": "To retrieve an object",
            "documentation": "The following example retrieves an object for an S3 bucket.",
            "input": {
              "Bucket": "examplebucket",
              "Key": "HappyFace.jpg"
            },
            "output": {
              "AcceptRanges": "bytes",
              "ContentType": "image/jpeg",
              "LastModified": "2016-12-15T01:19:41.000Z",
              "ContentLength": 3191,
              "VersionId": "null",
              "ETag": "\"6805f2cfc46c0f04559748bb039d69ae\"",
              "TagCount": 2,
              "Metadata": {}
            }
          }
        ],
        "smithy.api#http": {
          "method": "GET",
          "uri": "/{Bucket}/{Key+}?x-id=GetObject",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#GetObjectOutput": {
      "type": "structure",
      "members": {
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
        "DeleteMarker": {
          "target": "com.amazonaws.s3#DeleteMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-delete-marker"
            },
            "smithy.api#httpHeader": "x-amz-delete-marker"
          }
        },
        "AcceptRanges": {
          "target": "com.amazonaws.s3#AcceptRanges",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-accept-ranges"
            },
            "smithy.api#httpHeader": "accept-ranges"
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
        "Restore": {
          "target": "com.amazonaws.s3#Restore",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-restore"
            },
            "smithy.api#httpHeader": "x-amz-restore"
          }
        },
        "LastModified": {
          "target": "com.amazonaws.s3#LastModified",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-last-modified"
            },
            "smithy.api#httpHeader": "Last-Modified"
          }
        },
        "ContentLength": {
          "target": "com.amazonaws.s3#ContentLength",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-length"
            },
            "smithy.api#httpHeader": "Content-Length"
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
        "MissingMeta": {
          "target": "com.amazonaws.s3#MissingMeta",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-missing-meta"
            },
            "smithy.api#httpHeader": "x-amz-missing-meta"
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
        "CacheControl": {
          "target": "com.amazonaws.s3#CacheControl",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-cache-control"
            },
            "smithy.api#httpHeader": "Cache-Control"
          }
        },
        "ContentDisposition": {
          "target": "com.amazonaws.s3#ContentDisposition",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-disposition"
            },
            "smithy.api#httpHeader": "Content-Disposition"
          }
        },
        "ContentEncoding": {
          "target": "com.amazonaws.s3#ContentEncoding",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-encoding"
            },
            "smithy.api#httpHeader": "Content-Encoding"
          }
        },
        "ContentLanguage": {
          "target": "com.amazonaws.s3#ContentLanguage",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-language"
            },
            "smithy.api#httpHeader": "Content-Language"
          }
        },
        "ContentRange": {
          "target": "com.amazonaws.s3#ContentRange",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-range"
            },
            "smithy.api#httpHeader": "Content-Range"
          }
        },
        "ContentType": {
          "target": "com.amazonaws.s3#ContentType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-type"
            },
            "smithy.api#httpHeader": "Content-Type"
          }
        },
        "Expires": {
          "target": "com.amazonaws.s3#Expires",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-expires"
            },
            "smithy.api#httpHeader": "Expires"
          }
        },
        "WebsiteRedirectLocation": {
          "target": "com.amazonaws.s3#WebsiteRedirectLocation",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-website-redirect-location"
            },
            "smithy.api#httpHeader": "x-amz-website-redirect-location"
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
        "Metadata": {
          "target": "com.amazonaws.s3#Metadata",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-meta-*"
            },
            "smithy.api#httpPrefixHeaders": "x-amz-meta-"
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
        "BucketKeyEnabled": {
          "target": "com.amazonaws.s3#BucketKeyEnabled",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption-bucket-key-enabled"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption-bucket-key-enabled"
          }
        },
        "StorageClass": {
          "target": "com.amazonaws.s3#StorageClass",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-storage-class"
            },
            "smithy.api#httpHeader": "x-amz-storage-class"
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
        },
        "ReplicationStatus": {
          "target": "com.amazonaws.s3#ReplicationStatus",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-replication-status"
            },
            "smithy.api#httpHeader": "x-amz-replication-status"
          }
        },
        "PartsCount": {
          "target": "com.amazonaws.s3#PartsCount",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-mp-parts-count"
            },
            "smithy.api#httpHeader": "x-amz-mp-parts-count"
          }
        },
        "TagCount": {
          "target": "com.amazonaws.s3#TagCount",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-tagging-count"
            },
            "smithy.api#httpHeader": "x-amz-tagging-count"
          }
        },
        "ObjectLockMode": {
          "target": "com.amazonaws.s3#ObjectLockMode",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-object-lock-mode"
            },
            "smithy.api#httpHeader": "x-amz-object-lock-mode"
          }
        },
        "ObjectLockRetainUntilDate": {
          "target": "com.amazonaws.s3#ObjectLockRetainUntilDate",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-object-lock-retain-until-date"
            },
            "smithy.api#httpHeader": "x-amz-object-lock-retain-until-date"
          }
        },
        "ObjectLockLegalHoldStatus": {
          "target": "com.amazonaws.s3#ObjectLockLegalHoldStatus",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-object-lock-legal-hold"
            },
            "smithy.api#httpHeader": "x-amz-object-lock-legal-hold"
          }
        }
      },
      "traits": {
        "smithy.api#output": {}
      }
    },
    "com.amazonaws.s3#GetObjectRequest": {
      "type": "structure",
      "members": {
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
        "IfMatch": {
          "target": "com.amazonaws.s3#IfMatch",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-if-match"
            },
            "smithy.api#httpHeader": "If-Match"
          }
        },
        "IfModifiedSince": {
          "target": "com.amazonaws.s3#IfModifiedSince",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-if-modified-since"
            },
            "smithy.api#httpHeader": "If-Modified-Since"
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
        "IfUnmodifiedSince": {
          "target": "com.amazonaws.s3#IfUnmodifiedSince",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-if-unmodified-since"
            },
            "smithy.api#httpHeader": "If-Unmodified-Since"
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
        "Range": {
          "target": "com.amazonaws.s3#Range",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-range"
            },
            "smithy.api#httpHeader": "Range"
          }
        },
        "ResponseCacheControl": {
          "target": "com.amazonaws.s3#ResponseCacheControl",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-response-cache-control"
            },
            "smithy.api#httpQuery": "response-cache-control"
          }
        },
        "ResponseContentDisposition": {
          "target": "com.amazonaws.s3#ResponseContentDisposition",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-response-content-disposition"
            },
            "smithy.api#httpQuery": "response-content-disposition"
          }
        },
        "ResponseContentEncoding": {
          "target": "com.amazonaws.s3#ResponseContentEncoding",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-response-content-encoding"
            },
            "smithy.api#httpQuery": "response-content-encoding"
          }
        },
        "ResponseContentLanguage": {
          "target": "com.amazonaws.s3#ResponseContentLanguage",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-response-content-language"
            },
            "smithy.api#httpQuery": "response-content-language"
          }
        },
        "ResponseContentType": {
          "target": "com.amazonaws.s3#ResponseContentType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-response-content-type"
            },
            "smithy.api#httpQuery": "response-content-type"
          }
        },
        "ResponseExpires": {
          "target": "com.amazonaws.s3#ResponseExpires",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-response-expires"
            },
            "smithy.api#httpQuery": "response-expires"
          }
        },
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-versionid"
            },
            "smithy.api#httpQuery": "versionId"
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
        "RequestPayer": {
          "target": "com.amazonaws.s3#RequestPayer",
          "traits": {
            "smithy.api#httpHeader": "x-amz-request-payer",
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-request-payer"
            }
          }
        },
        "PartNumber": {
          "target": "com.amazonaws.s3#PartNumber",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-partnumber"
            },
            "smithy.api#httpQuery": "partNumber"
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
        "ChecksumMode": {
          "target": "com.amazonaws.s3#ChecksumMode",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-mode"
            },
            "smithy.api#httpHeader": "x-amz-checksum-mode"
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
