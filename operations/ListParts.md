# ListParts

[Request](#request) → [Response](#response-200-success)

Lists the parts that have been uploaded for a specific multipart upload.

**Multipart Workflow:** To list the parts, the requester must specify the upload identifier (`uploadId`) obtained during the initialization of the multipart upload (via `CreateMultipartUpload`). The returned parts are sorted by part number.

**Pagination:** By default, this operation returns up to 1,000 parts. If there are more parts available, the response includes an `IsTruncated` field set to `true` and a `NextPartNumberMarker`. To retrieve the subsequent parts, make a follow-up request with the `part-number-marker` parameter set to the value of `NextPartNumberMarker`. Requesters can also restrict the page size using the `max-parts` query parameter.

**Permissions:** The client credentials must be authorized to perform this operation on the target bucket. Under the standard S3 IAM policy model, this requires the `s3:ListMultipartUploadParts` action.

## Request

```HTTP
GET /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`

The name of the bucket associated with the multipart upload.

### Path: `key`

The object key for which the multipart upload was initiated.

### Query: `max-parts`

Sets the maximum number of parts to return in the response.

### Query: `part-number-marker`

Specifies the part number after which the listing should begin. Only parts with higher part numbers will be returned.

### Query: `uploadId`

The upload identifier that uniquely identifies the multipart upload.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-server-side-encryption-customer-algorithm`

Specifies the server-side encryption algorithm (e.g., `AES256`) used to encrypt the object. This parameter must be provided if the multipart upload was initiated using customer-provided encryption keys (SSE-C).

### Header: `x-amz-server-side-encryption-customer-key`

Specifies the customer-provided encryption key. This key must match the key used when the multipart upload was initiated.

### Header: `x-amz-server-side-encryption-customer-key-md5`

Provides the MD5 digest of the customer-provided encryption key for integrity validation.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<ListPartsResult>
	<Bucket>string</Bucket>
	<Key>string</Key>
	<UploadId>string</UploadId>
	<PartNumberMarker>string</PartNumberMarker>
	<NextPartNumberMarker>string</NextPartNumberMarker>
	<MaxParts>integer</MaxParts>
	<IsTruncated>boolean</IsTruncated>
	<Part>
		<PartNumber>integer</PartNumber>
		<LastModified>timestamp</LastModified>
		<ETag>string</ETag>
		<Size>integer</Size>
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
	</Part>
	<Initiator>
		<ID>string</ID>
		<DisplayName>string</DisplayName>
	</Initiator>
	<Owner>
		<DisplayName>string</DisplayName>
		<ID>string</ID>
	</Owner>
	<StorageClass>string</StorageClass>
	<ChecksumAlgorithm>string</ChecksumAlgorithm>
	<ChecksumType>string</ChecksumType>
</ListPartsResult>
```

### Response Header: `x-amz-abort-date`

If a lifecycle configuration rule matches the object and is set to abort incomplete multipart uploads, this header indicates when the initiated multipart upload becomes eligible for deletion.

### Response Header: `x-amz-abort-rule-id`

Indicates the ID of the lifecycle rule that defines the abort action for incomplete multipart uploads.

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Body: `<ListPartsResult>`

#### `<Bucket>`

The name of the bucket to which the multipart upload was initiated.

#### `<Key>`

The object key for which the multipart upload was initiated.

#### `<UploadId>`

The upload identifier that uniquely identifies the multipart upload.

#### `<PartNumberMarker>`

The part number marker specifying where the listing begins. Only parts with a number strictly greater than this marker are returned.

#### `<NextPartNumberMarker>`

When the response is truncated, this specifies the part number marker to use in subsequent requests to retrieve the next page of parts.

#### `<MaxParts>`

The maximum number of parts returned in this response.

#### `<IsTruncated>`

Indicates whether the returned list of parts is truncated. If `true`, more parts are available to be listed.

#### `<Part>`

Container for elements related to a specific part. A response can contain zero or more `Part` elements.
Type: Array of [Part](shared-shapes.md#schema-part)

#### `<Initiator>`

Identifies the user or account that initiated the multipart upload.
Type: [Initiator](shared-shapes.md#schema-initiator)

#### `<Owner>`

Identifies the owner of the object once the upload is completed.
Type: [Owner](shared-shapes.md#schema-owner)

#### `<StorageClass>`

The storage class to be applied to the completed object.

#### `<ChecksumAlgorithm>`

The algorithm used to compute the checksum for the object, if configured.

#### `<ChecksumType>`

The method used to combine individual part checksums into an object-level checksum.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#ListParts": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#ListPartsRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#ListPartsOutput"
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#listparts"
        },
        "smithy.api#examples": [
          {
            "title": "To list parts of a multipart upload.",
            "documentation": "The following example lists parts uploaded for a specific multipart upload.",
            "input": {
              "Bucket": "examplebucket",
              "Key": "bigobject",
              "UploadId": "example7YPBOJuoFiQ9cz4P3Pe6FIZwO4f7wN93uHsNBEw97pl5eNwzExg0LAT2dUN91cOmrEQHDsP3WA60CEg--"
            },
            "output": {
              "Owner": {
                "DisplayName": "owner-display-name",
                "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
              },
              "Initiator": {
                "DisplayName": "owner-display-name",
                "ID": "examplee7a2f25102679df27bb0ae12b3f85be6f290b936c4393484be31bebcc"
              },
              "Parts": [
                {
                  "LastModified": "2016-12-16T00:11:42.000Z",
                  "PartNumber": 1,
                  "ETag": "\"d8c2eafd90c266e19ab9dcacc479f8af\"",
                  "Size": 26246026
                },
                {
                  "LastModified": "2016-12-16T00:15:01.000Z",
                  "PartNumber": 2,
                  "ETag": "\"d8c2eafd90c266e19ab9dcacc479f8af\"",
                  "Size": 26246026
                }
              ],
              "StorageClass": "STANDARD"
            }
          }
        ],
        "smithy.api#http": {
          "method": "GET",
          "uri": "/{Bucket}/{Key+}?x-id=ListParts",
          "code": 200
        },
        "smithy.api#paginated": {
          "inputToken": "PartNumberMarker",
          "outputToken": "NextPartNumberMarker",
          "items": "Parts",
          "pageSize": "MaxParts"
        }
      }
    },
    "com.amazonaws.s3#ListPartsOutput": {
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
        "UploadId": {
          "target": "com.amazonaws.s3#MultipartUploadId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#uploadid"
            }
          }
        },
        "PartNumberMarker": {
          "target": "com.amazonaws.s3#PartNumberMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#partnumbermarker"
            }
          }
        },
        "NextPartNumberMarker": {
          "target": "com.amazonaws.s3#NextPartNumberMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#nextpartnumbermarker"
            }
          }
        },
        "MaxParts": {
          "target": "com.amazonaws.s3#MaxParts",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#maxparts"
            }
          }
        },
        "IsTruncated": {
          "target": "com.amazonaws.s3#IsTruncated",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#istruncated"
            }
          }
        },
        "Parts": {
          "target": "com.amazonaws.s3#Parts",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#part"
            },
            "smithy.api#xmlFlattened": {},
            "smithy.api#xmlName": "Part"
          }
        },
        "Initiator": {
          "target": "com.amazonaws.s3#Initiator",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#initiator"
            }
          }
        },
        "Owner": {
          "target": "com.amazonaws.s3#Owner",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#owner"
            }
          }
        },
        "StorageClass": {
          "target": "com.amazonaws.s3#StorageClass",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#storageclass"
            }
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
              "$ref": "#checksumalgorithm"
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
        }
      },
      "traits": {
        "smithy.api#output": {},
        "smithy.api#xmlName": "ListPartsResult"
      }
    },
    "com.amazonaws.s3#ListPartsRequest": {
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
        "MaxParts": {
          "target": "com.amazonaws.s3#MaxParts",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-max-parts"
            },
            "smithy.api#httpQuery": "max-parts"
          }
        },
        "PartNumberMarker": {
          "target": "com.amazonaws.s3#PartNumberMarker",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#query-part-number-marker"
            },
            "smithy.api#httpQuery": "part-number-marker"
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
