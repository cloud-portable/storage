# CompleteMultipartUpload

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Completes a multipart upload by assembling previously uploaded parts into a single object.

**Upload Assembling:** You first initiate the multipart upload and then upload all parts using the `UploadPart` or `UploadPartCopy` operations. After successfully uploading all parts, you call this operation to complete the upload. The storage engine concatenates all parts in ascending order by part number to create the final object. In the request body, you must provide a complete list of all parts, including the part number and the ETag returned by each individual upload.

**Delayed Error Handling:** The completion process might take several minutes to finalize. The storage engine may return an initial HTTP `200 OK` header to keep the connection alive while processing the assembly. White spaces may be sent periodically to prevent client timeouts. However, the assembly can still fail after this initial response is sent. Clients must parse the entire XML response body to verify that the assembly succeeded and did not contain an embedded error. Applications should be prepared to retry failed requests (including HTTP `500` internal errors).

## Request

```HTTP
POST /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`
The name of the bucket to which the multipart upload was initiated.

### Path: `key`
The object key of the multipart upload to complete.

### Query: `uploadId`
The unique identifier of the multipart upload to complete.

### Header: `x-amz-checksum-crc32`
The Base64-encoded, 32-bit CRC32 checksum of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-crc32c`
The Base64-encoded, 32-bit CRC32C checksum of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-crc64nvme`
The Base64-encoded, 64-bit CRC64NVME checksum of the assembled object, representing the full-object checksum.

### Header: `x-amz-checksum-sha1`
The Base64-encoded, 160-bit SHA1 digest of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-sha256`
The Base64-encoded, 256-bit SHA256 digest of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-sha512`
The Base64-encoded, 512-bit SHA512 digest of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-md5`
The Base64-encoded, 128-bit MD5 digest of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-xxhash64`
The Base64-encoded, 64-bit XXHASH64 checksum of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-xxhash3`
The Base64-encoded, 64-bit XXHASH3 checksum of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-xxhash128`
The Base64-encoded, 128-bit XXHASH128 checksum of the assembled object, used for data integrity verification.

### Header: `x-amz-checksum-type`
Specifies how part-level checksums are combined to create the object-level checksum. If this does not match the checksum type specified during upload initiation, the request will fail with a `BadDigest` error.

### Header: `x-amz-mp-object-size`
The expected total size of the final assembled object. If there is a mismatch between this value and the actual size of the assembled parts, an HTTP `400 Bad Request` error is returned.

### Header: `x-amz-request-payer`
Confirms that the requester understands they will be charged for processing the request. This header is typically required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`
The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `if-match`
Uploads the object only if the ETag value matches the ETag of the destination object. If the values do not match, the operation returns a `412 Precondition Failed` error.

If a conflict occurs, the operation returns a `409 ConditionalRequestConflict` response.

### Header: `if-none-match`
Uploads the object only if the object key does not already exist in the bucket. Otherwise, the operation returns a `412 Precondition Failed` error.

If a conflict occurs, the operation returns a `409 ConditionalRequestConflict` response.

### Header: `x-amz-server-side-encryption-customer-algorithm`
Specifies the encryption algorithm to use if the object was created using customer-provided encryption keys (SSE-C).

### Header: `x-amz-server-side-encryption-customer-key`
Specifies the 256-bit, base64-encoded customer encryption key if using SSE-C.

### Header: `x-amz-server-side-encryption-customer-key-md5`
Specifies the 128-bit, base64-encoded MD5 digest of the customer encryption key if using SSE-C.

### Body: `<CompleteMultipartUpload>`
The container for the multipart upload completion parameters.

Type: [CompletedMultipartUpload](shared-shapes.md#schema-completedmultipartupload)

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult>
	<Location>string</Location>
	<Bucket>string</Bucket>
	<Key>string</Key>
	<ETag>string</ETag>
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
	<ChecksumType>string</ChecksumType>
</CompleteMultipartUploadResult>
```

### Response Header: `x-amz-expiration`
If the object is subject to a lifecycle expiration rule, this contains the expiration date and matching rule ID.

### Response Header: `x-amz-server-side-encryption`
The server-side encryption algorithm used to store the assembled object (e.g., `AES256` or `aws:kms`).

### Response Header: `x-amz-version-id`
The unique version identifier of the newly created object, if versioning is enabled on the bucket.

### Response Header: `x-amz-server-side-encryption-aws-kms-key-id`
If SSE-KMS was used, this indicates the identifier of the KMS key used to encrypt the object.

### Response Header: `x-amz-server-side-encryption-bucket-key-enabled`
Indicates whether the object uses an S3 Bucket Key for SSE-KMS to reduce KMS request volume.

### Response Header: `x-amz-request-charged`
Confirms whether the requester was billed for the request. This header is returned in the response if the request was charged under requester-pays billing rules.

### Body: `<CompleteMultipartUploadResult>`

#### `<Location>`
The URI that identifies the newly created object.

#### `<Bucket>`
The name of the bucket that contains the newly created object.

#### `<Key>`
The object key of the newly created object.

#### `<ETag>`
The Entity Tag (ETag) of the newly created object, representing an opaque validator for the object data. For single-part uploads, this is the MD5 checksum of the payload. For multipart uploads, it is typically the MD5 checksum of the concatenated part checksums, followed by the part count suffix (e.g. `"<hash>-N"`).

#### `<ChecksumCRC32>`
The Base64-encoded, 32-bit CRC32 checksum of the object, if calculated.

#### `<ChecksumCRC32C>`
The Base64-encoded, 32-bit CRC32C checksum of the object, if calculated.

#### `<ChecksumCRC64NVME>`
The Base64-encoded, 64-bit CRC64NVME checksum of the object, if calculated.

#### `<ChecksumSHA1>`
The Base64-encoded, 160-bit SHA1 digest of the object, if calculated.

#### `<ChecksumSHA256>`
The Base64-encoded, 256-bit SHA256 digest of the object, if calculated.

#### `<ChecksumSHA512>`
The Base64-encoded, 512-bit SHA512 digest of the object, if calculated.

#### `<ChecksumMD5>`
The Base64-encoded, 128-bit MD5 digest of the object, if calculated.

#### `<ChecksumXXHASH64>`
The Base64-encoded, 64-bit XXHASH64 checksum of the object, if calculated.

#### `<ChecksumXXHASH3>`
The Base64-encoded, 64-bit XXHASH3 checksum of the object, if calculated.

#### `<ChecksumXXHASH128>`
The Base64-encoded, 128-bit XXHASH128 checksum of the object, if calculated.

#### `<ChecksumType>`
The checksum type used to compute object-level checksums, if applicable.

## Errors

### `400` `EntityTooSmall`
Your proposed upload is smaller than the minimum allowed object size. Each part must be at least 5 MB in size, except the last part.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>EntityTooSmall</Code>
	<Message>Your proposed upload is smaller than the minimum allowed object size. Each part must be at least 5 MB in size, except the last part.</Message>
</Error>
```

### `400` `InvalidPart`
One or more of the specified parts could not be found. The part might not have been uploaded, or the specified ETag might not have matched the uploaded part's ETag.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>InvalidPart</Code>
	<Message>One or more of the specified parts could not be found. The part might not have been uploaded, or the specified ETag might not have matched the uploaded part's ETag.</Message>
</Error>
```

### `400` `InvalidPartOrder`
The list of parts was not in ascending order. The parts list must be specified in order by part number.

```HTTP
HTTP/1.1 400 Bad Request
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>InvalidPartOrder</Code>
	<Message>The list of parts was not in ascending order. The parts list must be specified in order by part number.</Message>
</Error>
```

### `404` `NoSuchUpload`
The specified multipart upload does not exist. The upload ID might be invalid, or the multipart upload might have been aborted or completed.

```HTTP
HTTP/1.1 404 Not Found
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<Error>
	<Code>NoSuchUpload</Code>
	<Message>The specified multipart upload does not exist. The upload ID might be invalid, or the multipart upload might have been aborted or completed.</Message>
</Error>
```

## Smithy Spec


<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#CompleteMultipartUpload": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#CompleteMultipartUploadRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#CompleteMultipartUploadOutput"
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#completemultipartupload"
        },
        "smithy.api#examples": [
          {
            "title": "To complete multipart upload",
            "documentation": "The following example completes a multipart upload.",
            "input": {
              "Bucket": "examplebucket",
              "Key": "bigobject",
              "MultipartUpload": {
                "Parts": [
                  {
                    "PartNumber": 1,
                    "ETag": "\"d8c2eafd90c266e19ab9dcacc479f8af\""
                  },
                  {
                    "PartNumber": 2,
                    "ETag": "\"d8c2eafd90c266e19ab9dcacc479f8af\""
                  }
                ]
              },
              "UploadId": "7YPBOJuoFiQ9cz4P3Pe6FIZwO4f7wN93uHsNBEw97pl5eNwzExg0LAT2dUN91cOmrEQHDsP3WA60CEg--"
            },
            "output": {
              "ETag": "\"4d9031c7644d8081c2829f4ea23c55f7-2\"",
              "Bucket": "acexamplebucket",
              "Location": "https://examplebucket.s3.<Region>.amazonaws.com/bigobject",
              "Key": "bigobject"
            }
          }
        ],
        "smithy.api#http": {
          "method": "POST",
          "uri": "/{Bucket}/{Key+}",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#CompleteMultipartUploadOutput": {
      "type": "structure",
      "members": {
        "Location": {
          "target": "com.amazonaws.s3#Location",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#location"
            }
          }
        },
        "Bucket": {
          "target": "com.amazonaws.s3#BucketName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#bucket"
            }
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
              "$ref": "#etag"
            }
          }
        },
        "ChecksumCRC32": {
          "target": "com.amazonaws.s3#ChecksumCRC32",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumcrc32"
            }
          }
        },
        "ChecksumCRC32C": {
          "target": "com.amazonaws.s3#ChecksumCRC32C",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumcrc32c"
            }
          }
        },
        "ChecksumCRC64NVME": {
          "target": "com.amazonaws.s3#ChecksumCRC64NVME",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumcrc64nvme"
            }
          }
        },
        "ChecksumSHA1": {
          "target": "com.amazonaws.s3#ChecksumSHA1",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumsha1"
            }
          }
        },
        "ChecksumSHA256": {
          "target": "com.amazonaws.s3#ChecksumSHA256",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumsha256"
            }
          }
        },
        "ChecksumSHA512": {
          "target": "com.amazonaws.s3#ChecksumSHA512",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumsha512"
            }
          }
        },
        "ChecksumMD5": {
          "target": "com.amazonaws.s3#ChecksumMD5",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksummd5"
            }
          }
        },
        "ChecksumXXHASH64": {
          "target": "com.amazonaws.s3#ChecksumXXHASH64",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumxxhash64"
            }
          }
        },
        "ChecksumXXHASH3": {
          "target": "com.amazonaws.s3#ChecksumXXHASH3",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumxxhash3"
            }
          }
        },
        "ChecksumXXHASH128": {
          "target": "com.amazonaws.s3#ChecksumXXHASH128",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumxxhash128"
            }
          }
        },
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#checksumtype"
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
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-version-id"
            },
            "smithy.api#httpHeader": "x-amz-version-id"
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
        "smithy.api#output": {},
        "smithy.api#xmlName": "CompleteMultipartUploadResult"
      }
    },
    "com.amazonaws.s3#CompleteMultipartUploadRequest": {
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
        "MultipartUpload": {
          "target": "com.amazonaws.s3#CompletedMultipartUpload",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#body-completemultipartupload"
            },
            "smithy.api#httpPayload": {},
            "smithy.api#xmlName": "CompleteMultipartUpload"
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
        "ChecksumType": {
          "target": "com.amazonaws.s3#ChecksumType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-checksum-type"
            },
            "smithy.api#httpHeader": "x-amz-checksum-type"
          }
        },
        "MpuObjectSize": {
          "target": "com.amazonaws.s3#MpuObjectSize",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-mp-object-size"
            },
            "smithy.api#httpHeader": "x-amz-mp-object-size"
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
