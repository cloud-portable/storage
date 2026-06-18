# CreateMultipartUpload

[Request](#request) → [Response](#response-200-success)

Initiates a multipart upload and returns an upload identifier (`UploadId`).

**Multipart Workflow:** Initiating a multipart upload is the first step in uploading an object in multiple parts. The returned `UploadId` must be specified in all subsequent part uploads using `UploadPart` or `UploadPartCopy`. The upload session remains active until it is finalized using `CompleteMultipartUpload` or cancelled using `AbortMultipartUpload`. Storage charges may apply for accumulated parts until the upload is completed or aborted.

**Storage Lifecycle:** If the target bucket has a lifecycle rule configured to abort incomplete multipart uploads, the session must be completed within the specified timeframe. Otherwise, the incomplete parts are automatically cleaned up by the storage engine to free up storage space.

**Permissions:** Requesters must have permission to perform write operations on the target bucket and key path. Under the standard S3 IAM policy model, this requires the `s3:PutObject` action. Additional key permissions (such as `kms:GenerateDataKey`) may be required if encrypting the object using server-side encryption with KMS keys.

## Request

```HTTP
POST /{bucket}/{key}?uploads HTTP/1.1
```

### Path: `bucket`

The name of the bucket where the multipart upload is initiated.

### Path: `key`

The object key for which the multipart upload is initiated.

### Header: `x-amz-acl`

The canned access control list (ACL) to apply to the object.

> **Avoid:** Canned ACLs are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

### Header: `cache-control`

Specifies caching behavior along the request/reply chain.

### Header: `content-disposition`

Specifies presentational information for the object.

### Header: `content-encoding`

Specifies content encodings applied to the object, indicating what decoding mechanisms must be applied to obtain the media-type referenced by the Content-Type header.

### Header: `content-language`

The language that the content is in.

### Header: `content-type`

A standard MIME type describing the format of the object data.

### Header: `expires`

The date and time at which the object is no longer cacheable.

### Header: `x-amz-grant-full-control`

Explicitly grants read, write, and ACL permissions to specified grantees.

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

### Header: `x-amz-grant-read`

Explicitly grants read permission to specified grantees.

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

### Header: `x-amz-grant-read-acp`

Explicitly grants read-ACP permission to specified grantees.

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

### Header: `x-amz-grant-write-acp`

Explicitly grants write-ACP permission to specified grantees.

> **Avoid:** Fine-grained ACL grant headers are legacy. Modern S3-compatible deployments typically disable ACLs in favor of bucket policies or IAM.

### Header: `x-amz-meta-*`

A map of custom metadata keys and values to store with the object.

### Header: `x-amz-server-side-encryption`

Specifies the server-side encryption algorithm to use when storing the object. Common values include `AES256` (SSE-S3) and `aws:kms` (SSE-KMS). If no encryption headers are specified, the storage engine typically falls back to the bucket's default encryption configuration.

### Header: `x-amz-storage-class`

Specifies the storage class to use for storing the newly created object. If not specified, the storage engine uses its default storage class (typically `STANDARD`).

### Header: `x-amz-website-redirect-location`

Specifies the redirect location for requests to this object when the bucket is configured as a website. The redirect target can be a relative path or an external URL.

### Header: `x-amz-server-side-encryption-customer-algorithm`

Specifies the algorithm to use for encrypting the object with customer-provided keys (e.g., `AES256`).

### Header: `x-amz-server-side-encryption-customer-key`

Specifies the 256-bit, base64-encoded customer encryption key if using SSE-C.

### Header: `x-amz-server-side-encryption-customer-key-md5`

Specifies the 128-bit, base64-encoded MD5 digest of the customer encryption key if using SSE-C.

### Header: `x-amz-server-side-encryption-aws-kms-key-id`

Specifies the identifier of the KMS key to use for server-side encryption. This can be a key ID, key ARN, or alias. This header is only applicable when `x-amz-server-side-encryption` is set to `aws:kms`.

### Header: `x-amz-server-side-encryption-context`

Specifies the KMS encryption context as a Base64-encoded JSON string containing key-value pairs.

### Header: `x-amz-server-side-encryption-bucket-key-enabled`

Specifies whether to enable an S3 Bucket Key for server-side encryption with KMS (SSE-KMS). Enabling this option can significantly reduce the volume of KMS requests by caching key metadata.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-tagging`

A set of tags to associate with the object, formatted as URL-encoded key-value pairs (e.g., `Key1=Value1&Key2=Value2`).

### Header: `x-amz-object-lock-mode`

Specifies the Object Lock retention mode (`RETENTION` or `COMPLIANCE`) to apply to the uploaded object.

### Header: `x-amz-object-lock-retain-until-date`

Specifies the timestamp (in ISO 8601 format) when the Object Lock retention period should expire.

### Header: `x-amz-object-lock-legal-hold`

Specifies whether to apply a legal hold (`ON` or `OFF`) to the uploaded object.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-checksum-algorithm`

Indicates the algorithm to use to calculate the checksum for the object (e.g., `CRC32`, `CRC32C`, `SHA1`, `SHA256`). Specifying this enables server-side checksum validation during part uploads.

### Header: `x-amz-checksum-type`

Indicates the checksum type to use for calculating the object's checksum value.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult>
	<Bucket>string</Bucket>
	<Key>string</Key>
	<UploadId>string</UploadId>
</InitiateMultipartUploadResult>
```

### Response Header: `x-amz-abort-date`

If a lifecycle configuration rule matches the object and is set to abort incomplete multipart uploads, this header indicates when the initiated multipart upload becomes eligible for deletion.

### Response Header: `x-amz-abort-rule-id`

Indicates the ID of the lifecycle rule that defines the abort action for incomplete multipart uploads.

### Response Header: `x-amz-server-side-encryption`

The server-side encryption algorithm used to protect the object (e.g., `AES256` or `aws:kms`).

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

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Response Header: `x-amz-checksum-algorithm`

The algorithm used to calculate the checksum of the object, if specified.

### Response Header: `x-amz-checksum-type`

The checksum type used to compute object-level checksums, if applicable.

### Body: `<InitiateMultipartUploadResult>`

#### `<Bucket>`

The name of the bucket to which the multipart upload was initiated.

#### `<Key>`

Object key for which the multipart upload was initiated.

#### `<UploadId>`

ID for the initiated multipart upload.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#CreateMultipartUpload": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#CreateMultipartUploadRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#CreateMultipartUploadOutput"
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#createmultipartupload"
        },
        "smithy.api#examples": [
          {
            "title": "To initiate a multipart upload",
            "documentation": "The following example initiates a multipart upload.",
            "input": {
              "Bucket": "examplebucket",
              "Key": "largeobject"
            },
            "output": {
              "Bucket": "examplebucket",
              "UploadId": "ibZBv_75gd9r8lH_gqXatLdxMVpAlj6ZQjEs.OwyF3953YdwbcQnMA2BLGn8Lx12fQNICtMw5KyteFeHw.Sjng--",
              "Key": "largeobject"
            }
          }
        ],
        "smithy.api#http": {
          "method": "POST",
          "uri": "/{Bucket}/{Key+}?uploads",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#CreateMultipartUploadOutput": {
      "type": "structure",
      "members": {
        "AbortDate": {
          "target": "com.amazonaws.s3#AbortDate",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-abort-date"
            },
            "smithy.api#httpHeader": "x-amz-abort-date"
          }
        },
        "AbortRuleId": {
          "target": "com.amazonaws.s3#AbortRuleId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-abort-rule-id"
            },
            "smithy.api#httpHeader": "x-amz-abort-rule-id"
          }
        },
        "Bucket": {
          "target": "com.amazonaws.s3#BucketName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#bucket"
            },
            "smithy.api#xmlName": "Bucket"
          }
        },
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#key"
            }
          }
        },
        "UploadId": {
          "target": "com.amazonaws.s3#MultipartUploadId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#uploadid"
            }
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
        },
        "ChecksumAlgorithm": {
          "target": "com.amazonaws.s3#ChecksumAlgorithm",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-checksum-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-checksum-algorithm"
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
        }
      },
      "traits": {
        "smithy.api#output": {},
        "smithy.api#xmlName": "InitiateMultipartUploadResult"
      }
    },
    "com.amazonaws.s3#CreateMultipartUploadRequest": {
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
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-type"
            },
            "smithy.api#httpHeader": "x-amz-checksum-type"
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
