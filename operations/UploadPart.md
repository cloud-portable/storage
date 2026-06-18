# UploadPart

[Request](#request) → [Response](#response-200-success)

Uploads a part in a multipart upload.

**Multipart Workflow:** To upload a part, the client must specify the bucket name, the object key, the upload identifier (`uploadId`), and a part number (ranging from 1 to 10,000, inclusive). The part number defines the part's position within the final object. Uploading a part with a part number that was previously used overwrites the existing part data.

**Billing & Lifecycle:** Requesters accumulate storage costs for all uploaded parts until the multipart upload is either finalized via `CompleteMultipartUpload` or cancelled via `AbortMultipartUpload`. It is recommended to configure lifecycle rules to clean up incomplete multipart uploads automatically.

**Permissions:** The client credentials must be authorized to perform this operation. Under the standard S3 IAM policy model, this requires the `s3:PutObject` action. If the upload is encrypted using server-side encryption with KMS keys, key permissions (e.g., `kms:Decrypt` and `kms:GenerateDataKey`) are also required.

## Request

```HTTP
PUT /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`

The name of the bucket associated with the multipart upload.

### Path: `key`

The object key for which the multipart upload was initiated.

### Query: `partNumber`

The part number identifying the position of the part. This must be an integer between 1 and 10,000, inclusive.

### Query: `uploadId`

The upload identifier that uniquely identifies the multipart upload.

### Header: `content-length`

Size of the body in bytes. This is useful when the size of the request body cannot be determined automatically.

### Header: `content-md5`

The Base64 encoded 128-bit MD5 digest of the part payload for integrity validation.

### Header: `x-amz-sdk-checksum-algorithm`

Specifies the checksum algorithm used to validate the part data.

When this header is specified, the client must also provide the matching checksum digest header, such as `x-amz-checksum-sha256`. The storage engine validates the payload against the provided digest and returns a `BadDigest` error if they do not match.

### Header: `x-amz-checksum-crc32`

The Base64 encoded 32-bit CRC32 checksum of the part.

### Header: `x-amz-checksum-crc32c`

The Base64 encoded 32-bit CRC32C checksum of the part.

### Header: `x-amz-checksum-crc64nvme`

The Base64 encoded 64-bit CRC64NVME checksum of the part.

### Header: `x-amz-checksum-sha1`

The Base64 encoded 160-bit SHA1 digest of the part.

### Header: `x-amz-checksum-sha256`

The Base64 encoded 256-bit SHA256 digest of the part.

### Header: `x-amz-checksum-sha512`

The Base64 encoded 512-bit SHA512 digest of the part.

### Header: `x-amz-checksum-md5`

The Base64 encoded 128-bit MD5 checksum of the part.

### Header: `x-amz-checksum-xxhash64`

The Base64 encoded 64-bit XXHASH64 checksum of the part.

### Header: `x-amz-checksum-xxhash3`

The Base64 encoded 64-bit XXHASH3 checksum of the part.

### Header: `x-amz-checksum-xxhash128`

The Base64 encoded 128-bit XXHASH128 checksum of the part.

### Header: `x-amz-server-side-encryption-customer-algorithm`

Specifies the algorithm to use for encrypting the part data with customer-provided keys (e.g., `AES256`). This must match the algorithm specified when the multipart upload was initiated.

### Header: `x-amz-server-side-encryption-customer-key`

Specifies the customer-provided encryption key. This key must match the key used when the multipart upload was initiated.

### Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Body: `blob`

The payload data of the part being uploaded.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
```

### Response Header: `x-amz-server-side-encryption`

The server-side encryption algorithm used to protect the object (e.g., `AES256` or `aws:kms`).

### Response Header: `etag`

The entity tag (ETag) representing the uploaded part, typically formatted as the double-quoted MD5 hash of the part payload.

### Response Header: `x-amz-checksum-crc32`

The Base64 encoded 32-bit CRC32 checksum of the part.

### Response Header: `x-amz-checksum-crc32c`

The Base64 encoded 32-bit CRC32C checksum of the part.

### Response Header: `x-amz-checksum-crc64nvme`

The Base64 encoded 64-bit CRC64NVME checksum of the part.

### Response Header: `x-amz-checksum-sha1`

The Base64 encoded 160-bit SHA1 digest of the part.

### Response Header: `x-amz-checksum-sha256`

The Base64 encoded 256-bit SHA256 digest of the part.

### Response Header: `x-amz-checksum-sha512`

The Base64 encoded 512-bit SHA512 digest of the part.

### Response Header: `x-amz-checksum-md5`

The Base64 encoded 128-bit MD5 checksum of the part.

### Response Header: `x-amz-checksum-xxhash64`

The Base64 encoded 64-bit XXHASH64 checksum of the part.

### Response Header: `x-amz-checksum-xxhash3`

The Base64 encoded 64-bit XXHASH3 checksum of the part.

### Response Header: `x-amz-checksum-xxhash128`

The Base64 encoded 128-bit XXHASH128 checksum of the part.

### Response Header: `x-amz-server-side-encryption-customer-algorithm`

Confirms the server-side encryption algorithm used when customer-provided encryption keys (SSE-C) were requested.

### Response Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation when SSE-C was used.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`

The KMS key identifier used for encrypting the object, if SSE-KMS was requested.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`

Indicates whether an S3 Bucket Key was used for SSE-KMS encryption.

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#UploadPart": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#UploadPartRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#UploadPartOutput"
      },
      "traits": {
        "aws.protocols#httpChecksum": {
          "requestAlgorithmMember": "ChecksumAlgorithm"
        },
        "smithy.api#documentation": {
          "$ref": "#uploadpart"
        },
        "smithy.api#examples": [
          {
            "title": "To upload a part",
            "documentation": "The following example uploads part 1 of a multipart upload. The example specifies a file name for the part data. The Upload ID is same that is returned by the initiate multipart upload.",
            "input": {
              "Body": "fileToUpload",
              "Bucket": "examplebucket",
              "Key": "examplelargeobject",
              "PartNumber": 1,
              "UploadId": "xadcOB_7YPBOJuoFiQ9cz4P3Pe6FIZwO4f7wN93uHsNBEw97pl5eNwzExg0LAT2dUN91cOmrEQHDsP3WA60CEg--"
            },
            "output": {
              "ETag": "\"d8c2eafd90c266e19ab9dcacc479f8af\""
            }
          }
        ],
        "smithy.api#http": {
          "method": "PUT",
          "uri": "/{Bucket}/{Key+}?x-id=UploadPart",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#UploadPartOutput": {
      "type": "structure",
      "members": {
        "ServerSideEncryption": {
          "target": "com.amazonaws.s3#ServerSideEncryption",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-server-side-encryption"
            },
            "smithy.api#httpHeader": "x-amz-server-side-encryption"
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
    "com.amazonaws.s3#UploadPartRequest": {
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
