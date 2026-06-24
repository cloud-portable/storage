# UploadPartCopy

[Request](#request) → [Response](#response-200-success)

Uploads a part of an in-progress multipart upload by copying data from an existing object as the data source.

**Multipart Workflow:** To upload a part via copy, the client must specify the target bucket name, the target object key, the upload identifier (`uploadId`), the part number (ranging from 1 to 10,000, inclusive), and the copy source object (`x-amz-copy-source`). Clients can optionally copy a specific byte range of the source object by providing the `x-amz-copy-source-range` header.

**Source Object Access:** By default, this operation copies the current version of the source object. If versioning is enabled on the source bucket, a specific version can be copied by appending `?versionId=<version-id>` to the copy source path. If the source object is encrypted, appropriate decryption keys must be provided.

**Permissions:** The client credentials must be authorized to perform this operation. This requires read permissions (`s3:GetObject` in the standard S3 IAM policy model) on the copy source object, and write permissions (`s3:PutObject` in the standard S3 IAM policy model) on the destination bucket. If the source or destination is encrypted with KMS keys, key permissions (e.g., `kms:Decrypt` and `kms:GenerateDataKey`) are also required.

## Request

```HTTP
PUT /{bucket}/{key} HTTP/1.1
x-amz-copy-source: {CopySource}
```

### Path: `bucket`

The name of the destination bucket for the multipart upload.

### Path: `key`

The destination object key for which the multipart upload was initiated.

### Query: `partNumber`

The part number identifying the position of the part. This must be an integer between 1 and 10,000, inclusive.

### Query: `uploadId`

The upload identifier that uniquely identifies the multipart upload.

### Header: `x-amz-copy-source`

Specifies the source object to copy, formatted as `/bucket-name/object-key`.

To copy a specific version of the source object, append `?versionId=<version-id>` to the path. If the source object is a delete marker, copying without specifying a version ID returns a `404 Not Found` error.

### Header: `x-amz-copy-source-if-match`

Copies the object only if its ETag matches the specified ETag.

### Header: `x-amz-copy-source-if-modified-since`

Copies the object only if it has been modified since the specified timestamp.

### Header: `x-amz-copy-source-if-none-match`

Copies the object only if its ETag does not match the specified ETag.

### Header: `x-amz-copy-source-if-unmodified-since`

Copies the object only if it has not been modified since the specified timestamp.

### Header: `x-amz-copy-source-range`

Specifies the byte range to copy from the source object, formatted as `bytes=first-last` (zero-based).

### Header: `x-amz-server-side-encryption-customer-algorithm`

Specifies the server-side encryption algorithm to use for encrypting the destination part with customer-provided keys (e.g., `AES256`).

### Header: `x-amz-server-side-encryption-customer-key`

Specifies the customer-provided encryption key for the destination part.

### Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the destination customer-provided encryption key for integrity validation.

### Header: `x-amz-copy-source-server-side-encryption-customer-algorithm`

Specifies the algorithm used to decrypt the source object (e.g., `AES256`).

### Header: `x-amz-copy-source-server-side-encryption-customer-key`

Specifies the customer-provided encryption key required to decrypt the source object.

### Header: `x-amz-copy-source-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the source customer-provided encryption key for integrity validation.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected destination bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-source-expected-bucket-owner`

The account identifier of the expected source bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<CopyPartResult>
	<ETag>string</ETag>
	<LastModified>timestamp</LastModified>
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
</CopyPartResult>
```

### Response Header: `x-amz-copy-source-version-id`

The version ID of the source object that was copied, if versioning is enabled on the source bucket.

### Response Header: `x-amz-server-side-encryption`

The server-side encryption algorithm used to protect the destination part (e.g., `AES256` or `aws:kms`).

### Response Header: `x-amz-server-side-encryption-customer-algorithm`

Confirms the server-side encryption algorithm used when customer-provided encryption keys (SSE-C) were requested for the destination part.

### Response Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation when SSE-C was used for the destination part.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`

The KMS key identifier used for encrypting the destination part, if SSE-KMS was requested.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`

Indicates whether an S3 Bucket Key was used for SSE-KMS encryption.

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Body: `<CopyPartResult>`

Container for all response elements.
Type: [CopyPartResult](shared-shapes.md#schema-copypartresult)

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#UploadPartCopy": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#UploadPartCopyRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#UploadPartCopyOutput"
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#uploadpartcopy"
        },
        "smithy.api#examples": [
          {
            "title": "To upload a part by copying byte range from an existing object as data source",
            "documentation": "The following example uploads a part of a multipart upload by copying a specified byte range from an existing object as data source.",
            "input": {
              "Bucket": "examplebucket",
              "CopySource": "/bucketname/sourceobjectkey",
              "CopySourceRange": "bytes=1-100000",
              "Key": "examplelargeobject",
              "PartNumber": 2,
              "UploadId": "exampleuoh_10OhKhT7YukE9bjzTPRiuaCotmZM_pFngJFir9OZNrSr5cWa3cq3LZSUsfjI4FI7PkP91We7Nrw--"
            },
            "output": {
              "CopyPartResult": {
                "LastModified": "2016-12-29T21:44:28.000Z",
                "ETag": "\"65d16d19e65a7508a51f043180edcc36\""
              }
            }
          },
          {
            "title": "To upload a part by copying data from an existing object as data source",
            "documentation": "The following example uploads a part of a multipart upload by copying data from an existing object as data source.",
            "input": {
              "Bucket": "examplebucket",
              "CopySource": "/bucketname/sourceobjectkey",
              "Key": "examplelargeobject",
              "PartNumber": 1,
              "UploadId": "exampleuoh_10OhKhT7YukE9bjzTPRiuaCotmZM_pFngJFir9OZNrSr5cWa3cq3LZSUsfjI4FI7PkP91We7Nrw--"
            },
            "output": {
              "CopyPartResult": {
                "LastModified": "2016-12-29T21:24:43.000Z",
                "ETag": "\"b0c6f0e7e054ab8fa2536a2677f8734d\""
              }
            }
          }
        ],
        "smithy.api#http": {
          "method": "PUT",
          "uri": "/{Bucket}/{Key+}?x-id=UploadPartCopy",
          "code": 200
        },
        "smithy.rules#staticContextParams": {
          "DisableS3ExpressSessionAuth": {
            "value": true
          }
        }
      }
    },
    "com.amazonaws.s3#UploadPartCopyOutput": {
      "type": "structure",
      "members": {
        "CopySourceVersionId": {
          "target": "com.amazonaws.s3#CopySourceVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-copy-source-version-id"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-version-id"
          }
        },
        "CopyPartResult": {
          "target": "com.amazonaws.s3#CopyPartResult",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#body-copypartresult"
            },
            "smithy.api#httpPayload": {}
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
    "com.amazonaws.s3#UploadPartCopyRequest": {
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
        "CopySource": {
          "target": "com.amazonaws.s3#CopySource",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source"
            },
            "smithy.api#httpHeader": "x-amz-copy-source",
            "smithy.api#required": {}
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
        "CopySourceRange": {
          "target": "com.amazonaws.s3#CopySourceRange",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-copy-source-range"
            },
            "smithy.api#httpHeader": "x-amz-copy-source-range"
          }
        },
        "Key": {
          "target": "com.amazonaws.s3#ObjectKey",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#path-key"
            },
            "smithy.api#httpLabel": {},
            "smithy.api#required": {}
          }
        },
        "PartNumber": {
          "target": "com.amazonaws.s3#PartNumber",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-partnumber"
            },
            "smithy.api#httpQuery": "partNumber",
            "smithy.api#required": {}
          }
        },
        "UploadId": {
          "target": "com.amazonaws.s3#MultipartUploadId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-uploadid"
            },
            "smithy.api#httpQuery": "uploadId",
            "smithy.api#required": {}
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
