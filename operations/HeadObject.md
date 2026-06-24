# HeadObject

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Retrieves metadata from an object without returning the object payload itself.

**Metadata Retrieval:** This operation is useful for retrieving object properties (such as size, ETag, content type, or custom metadata) without transferring the object content. The response is identical to a `GET` request for the same object, except the response body is omitted. If the object does not exist or access is denied, the storage engine returns standard HTTP status codes (such as `404 Not Found` or `403 Forbidden`). Because this is a `HEAD` request, no response body is returned with error details.

**Versioning and Delete Markers:** If the requested key points to a delete marker, the storage engine treats the object as deleted, returning an HTTP `404 Not Found` response with the `x-amz-delete-marker: true` header. If a specific version ID is requested and it points to a delete marker, the response returns HTTP `405 Method Not Allowed`.

**Permissions:** Requesters must have permission to read the object. Under the standard S3 IAM policy model, this requires the `s3:GetObject` action. If the object does not exist, the error code returned depends on list permissions: an HTTP `404 Not Found` is returned if the requester has the `s3:ListBucket` action; otherwise, an HTTP `403 Forbidden` is returned.

## Request

```HTTP
HEAD /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`

The name of the bucket that contains the object.

### Path: `key`

The object key.

### Query: `response-cache-control`

Overrides the `Cache-Control` header in the response.

### Query: `response-content-disposition`

Overrides the `Content-Disposition` header in the response.

### Query: `response-content-encoding`

Overrides the `Content-Encoding` header in the response.

### Query: `response-content-language`

Overrides the `Content-Language` header in the response.

### Query: `response-content-type`

Overrides the `Content-Type` header in the response.

### Query: `response-expires`

Overrides the `Expires` header in the response.

### Query: `versionId`

Specifies the version identifier of the object to query. If not specified, the query applies to the current version.

### Query: `partNumber`

Specifies the part number of the object to query (a positive integer between 1 and 10,000). This performs a ranged query for the metadata of the specified part.

### Header: `if-match`

Returns object metadata only if its entity tag (ETag) matches the specified value. Otherwise, the operation returns an HTTP `412 Precondition Failed` error.

### Header: `if-modified-since`

Returns object metadata only if the object has been modified since the specified timestamp. Otherwise, the operation returns an HTTP `304 Not Modified` error.

### Header: `if-none-match`

Returns object metadata only if its entity tag (ETag) is different from the specified value. Otherwise, the operation returns an HTTP `304 Not Modified` error.

### Header: `if-unmodified-since`

Returns object metadata only if the object has not been modified since the specified timestamp. Otherwise, the operation returns an HTTP `412 Precondition Failed` error.

### Header: `range`

Specifies the byte range to query. If satisfiable, only the `Content-Length` header in the response is affected.

### Header: `x-amz-server-side-encryption-customer-algorithm`

Specifies the encryption algorithm used to encrypt the object with customer-provided keys (SSE-C).

### Header: `x-amz-server-side-encryption-customer-key`

Specifies the 256-bit, base64-encoded customer encryption key if using SSE-C.

### Header: `x-amz-server-side-encryption-customer-key-md5`

Specifies the 128-bit, base64-encoded MD5 digest of the customer encryption key if using SSE-C.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-checksum-mode`

Specifies whether to compute and return checksum values in the response (e.g. set to `ENABLED`).

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK

```

### Response Header: `x-amz-delete-marker`

Indicates whether the queried object version is a delete marker.

### Response Header: `accept-ranges`

Indicates whether the storage engine supports range requests for the object.

### Response Header: `x-amz-expiration`

If a lifecycle expiration rule applies to the object, this header indicates when the object will expire and the matching rule identifier.

### Response Header: `x-amz-restore`

Indicates the restoration status of the object if it is archived in a cold storage tier.

### Response Header: `x-amz-archive-status`

The archive status of the object if stored in a cold tier.

### Response Header: `last-modified`

The date and time when the object was last modified.

### Response Header: `content-length`

The size of the object body in bytes.

### Response Header: `x-amz-checksum-crc32`

The Base64-encoded, 32-bit CRC32 checksum of the object, if calculated.

### Response Header: `x-amz-checksum-crc32c`

The Base64-encoded, 32-bit CRC32C checksum of the object, if calculated.

### Response Header: `x-amz-checksum-crc64nvme`

The Base64-encoded, 64-bit CRC64NVME checksum of the object, if calculated.

### Response Header: `x-amz-checksum-sha1`

The Base64-encoded, 160-bit SHA1 digest of the object, if calculated.

### Response Header: `x-amz-checksum-sha256`

The Base64-encoded, 256-bit SHA256 digest of the object, if calculated.

### Response Header: `x-amz-checksum-sha512`

The Base64-encoded, 512-bit SHA512 digest of the object, if calculated.

### Response Header: `x-amz-checksum-md5`

The Base64-encoded, 128-bit MD5 digest of the object, if calculated.

### Response Header: `x-amz-checksum-xxhash64`

The Base64-encoded, 64-bit XXHASH64 checksum of the object, if calculated.

### Response Header: `x-amz-checksum-xxhash3`

The Base64-encoded, 64-bit XXHASH3 checksum of the object, if calculated.

### Response Header: `x-amz-checksum-xxhash128`

The Base64-encoded, 128-bit XXHASH128 checksum of the object, if calculated.

### Response Header: `x-amz-checksum-type`

The checksum type used to compute object-level checksums, if applicable.

### Response Header: `etag`

The Entity Tag (ETag) of the object, representing an opaque validator for the object data.

### Response Header: `x-amz-missing-meta`

Indicates the number of custom metadata headers that could not be returned due to size or naming limitations.

### Response Header: `x-amz-version-id`

The version identifier of the object.

### Response Header: `cache-control`

Specifies caching behavior along the request/reply chain.

### Response Header: `content-disposition`

Specifies presentational information for the object.

### Response Header: `content-encoding`

Indicates what content encodings have been applied to the object.

### Response Header: `content-language`

The language the content is in.

### Response Header: `content-type`

A standard MIME type describing the format of the object data.

### Response Header: `content-range`

The range of bytes returned by the request, if applicable.

### Response Header: `expires`

The date and time at which the object is no longer cacheable.

### Response Header: `x-amz-website-redirect-location`

Specifies the redirect location for requests to this object when the bucket is configured as a website.

### Response Header: `x-amz-server-side-encryption`

The server-side encryption algorithm used to protect the object (e.g., `AES256` or `aws:kms`).

### Response Header: `x-amz-meta-*`

A map of custom metadata keys and values associated with the object.

### Response Header: `x-amz-server-side-encryption-customer-algorithm`

Confirms the encryption algorithm used if the object was encrypted using customer-provided keys (SSE-C).

### Response Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation if SSE-C was used.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`

The KMS key identifier used for encrypting the object, if SSE-KMS was requested.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`

Indicates whether the object uses an S3 Bucket Key for SSE-KMS encryption.

### Response Header: `x-amz-storage-class`

Provides storage class information of the object.

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Response Header: `x-amz-replication-status`

Indicates the replication status of the object (e.g. `PENDING`, `COMPLETED`, `FAILED`, or `REPLICA`).

### Response Header: `x-amz-mp-parts-count`

The count of parts this object has. This is only returned if the object was uploaded as a multipart upload.

### Response Header: `x-amz-tagging-count`

The number of tags associated with the object.

### Response Header: `x-amz-object-lock-mode`

The Object Lock retention mode (`RETENTION` or `COMPLIANCE`) currently in effect for the object.

### Response Header: `x-amz-object-lock-retain-until-date`

The date and time when the Object Lock retention period is set to expire.

### Response Header: `x-amz-object-lock-legal-hold`

Specifies whether a legal hold is in effect for the object (`ON` or `OFF`).

## Errors

### `404` `NotFound`

The specified object does not exist.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#HeadObject": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#HeadObjectRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#HeadObjectOutput"
      },
      "errors": [
        {
          "target": "com.amazonaws.s3#NotFound"
        }
      ],
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#headobject"
        },
        "smithy.api#examples": [
          {
            "title": "To retrieve metadata of an object without returning the object itself",
            "documentation": "The following example retrieves an object metadata.",
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
              "Metadata": {}
            }
          }
        ],
        "smithy.api#http": {
          "method": "HEAD",
          "uri": "/{Bucket}/{Key+}",
          "code": 200
        },
        "smithy.waiters#waitable": {
          "ObjectExists": {
            "acceptors": [
              {
                "state": "success",
                "matcher": {
                  "success": true
                }
              },
              {
                "state": "retry",
                "matcher": {
                  "errorType": "NotFound"
                }
              }
            ],
            "minDelay": 5
          },
          "ObjectNotExists": {
            "acceptors": [
              {
                "state": "success",
                "matcher": {
                  "errorType": "NotFound"
                }
              }
            ],
            "minDelay": 5
          }
        }
      }
    },
    "com.amazonaws.s3#HeadObjectOutput": {
      "type": "structure",
      "members": {
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
        "ArchiveStatus": {
          "target": "com.amazonaws.s3#ArchiveStatus",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-archive-status"
            },
            "smithy.api#httpHeader": "x-amz-archive-status"
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
        "ETag": {
          "target": "com.amazonaws.s3#ETag",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-etag"
            },
            "smithy.api#httpHeader": "ETag"
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
        "ContentType": {
          "target": "com.amazonaws.s3#ContentType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-content-type"
            },
            "smithy.api#httpHeader": "Content-Type"
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
    "com.amazonaws.s3#HeadObjectRequest": {
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
