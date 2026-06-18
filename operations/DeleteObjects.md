# DeleteObjects

[Request](#request) → [Response](#response-200-success)

Deletes multiple objects from a bucket in a single HTTP request.

**Batch Operations:** This operation allows clients to delete up to 1,000 keys in a single transaction, significantly reducing request overhead compared to individual delete requests. The request body contains an XML list of the object keys to delete. For versioned buckets, specific version identifiers can also be specified to permanently delete those versions.

**Response Modes:** The operation supports two modes for returning results:
- **Verbose Mode (Default):** The response includes a detailed status (success or failure) for every key specified in the request.
- **Quiet Mode:** The response only includes entries for keys that encountered an error during deletion. Successful deletions do not return any information.

**Permissions:** Requesters must have permission to perform delete operations on the target bucket. Under the standard S3 IAM policy model, this requires the `s3:DeleteObject` action. Deleting specific object versions requires the `s3:DeleteObjectVersion` action. If any deletion attempt is denied by policy, the response will record a permission error for that key.

## Request

```HTTP
POST /{bucket}?delete HTTP/1.1
```

### Path: `bucket`

The name of the bucket containing the objects to delete.

### Header: `x-amz-mfa`

> **Avoid:** MFA delete configurations are legacy and unsupported by many engines.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-bypass-governance-retention`

Specifies whether to bypass Object Lock Governance-mode retention configurations to delete the object versions.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `x-amz-sdk-checksum-algorithm`

Indicates the algorithm to use to calculate the checksum for the request body (e.g., `CRC32`, `CRC32C`, `SHA1`, `SHA256`). Specifying this enables integrity validation of the delete list payload.

### Body: `<Delete>`

The container for the list of objects to delete and the quiet/verbose mode setting.

Type: [Delete](shared-shapes.md#schema-delete)

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?>
<DeleteResult>
	<Deleted>
		<Key>string</Key>
		<VersionId>string</VersionId>
		<DeleteMarker>boolean</DeleteMarker>
		<DeleteMarkerVersionId>string</DeleteMarkerVersionId>
	</Deleted>
	<Error>
		<Key>string</Key>
		<VersionId>string</VersionId>
		<Code>string</Code>
		<Message>string</Message>
	</Error>
</DeleteResult>
```

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

### Body: `<DeleteResult>`

#### `<Deleted>`

A container element indicating a successfully deleted object, including the key, version ID, and version deletion status.

Type: Array of [DeletedObject](shared-shapes.md#schema-deletedobject)

#### `<Error>`

A container element describing an object deletion that encountered an error, including the key, version ID, error code, and error message.

Type: Array of [Error](shared-shapes.md#schema-error)

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#DeleteObjects": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#DeleteObjectsRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#DeleteObjectsOutput"
      },
      "traits": {
        "aws.protocols#httpChecksum": {
          "requestAlgorithmMember": "ChecksumAlgorithm",
          "requestChecksumRequired": true
        },
        "smithy.api#documentation": {
          "$ref": "#deleteobjects"
        },
        "smithy.api#examples": [
          {
            "title": "To delete multiple object versions from a versioned bucket",
            "documentation": "The following example deletes objects from a bucket. The request specifies object versions. S3 deletes specific object versions and returns the key and versions of deleted objects in the response.",
            "input": {
              "Bucket": "examplebucket",
              "Delete": {
                "Objects": [
                  {
                    "Key": "HappyFace.jpg",
                    "VersionId": "2LWg7lQLnY41.maGB5Z6SWW.dcq0vx7b"
                  },
                  {
                    "Key": "HappyFace.jpg",
                    "VersionId": "yoz3HB.ZhCS_tKVEmIOr7qYyyAaZSKVd"
                  }
                ],
                "Quiet": false
              }
            },
            "output": {
              "Deleted": [
                {
                  "VersionId": "yoz3HB.ZhCS_tKVEmIOr7qYyyAaZSKVd",
                  "Key": "HappyFace.jpg"
                },
                {
                  "VersionId": "2LWg7lQLnY41.maGB5Z6SWW.dcq0vx7b",
                  "Key": "HappyFace.jpg"
                }
              ]
            }
          },
          {
            "title": "To delete multiple objects from a versioned bucket",
            "documentation": "The following example deletes objects from a bucket. The bucket is versioned, and the request does not specify the object version to delete. In this case, all versions remain in the bucket and S3 adds a delete marker.",
            "input": {
              "Bucket": "examplebucket",
              "Delete": {
                "Objects": [
                  {
                    "Key": "objectkey1"
                  },
                  {
                    "Key": "objectkey2"
                  }
                ],
                "Quiet": false
              }
            },
            "output": {
              "Deleted": [
                {
                  "DeleteMarkerVersionId": "A._w1z6EFiCF5uhtQMDal9JDkID9tQ7F",
                  "Key": "objectkey1",
                  "DeleteMarker": true
                },
                {
                  "DeleteMarkerVersionId": "iOd_ORxhkKe_e8G8_oSGxt2PjsCZKlkt",
                  "Key": "objectkey2",
                  "DeleteMarker": true
                }
              ]
            }
          }
        ],
        "smithy.api#http": {
          "method": "POST",
          "uri": "/{Bucket}?delete",
          "code": 200
        }
      }
    },
    "com.amazonaws.s3#DeleteObjectsOutput": {
      "type": "structure",
      "members": {
        "Deleted": {
          "target": "com.amazonaws.s3#DeletedObjects",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#deleted"
            },
            "smithy.api#xmlFlattened": {}
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
        "Errors": {
          "target": "com.amazonaws.s3#Errors",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#error"
            },
            "smithy.api#xmlFlattened": {},
            "smithy.api#xmlName": "Error"
          }
        }
      },
      "traits": {
        "smithy.api#output": {},
        "smithy.api#xmlName": "DeleteResult"
      }
    },
    "com.amazonaws.s3#DeleteObjectsRequest": {
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
        "Delete": {
          "target": "com.amazonaws.s3#Delete",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#body-delete"
            },
            "smithy.api#httpPayload": {},
            "smithy.api#required": {},
            "smithy.api#xmlName": "Delete"
          }
        },
        "MFA": {
          "target": "com.amazonaws.s3#MFA",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-mfa"
            },
            "smithy.api#httpHeader": "x-amz-mfa"
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
        "BypassGovernanceRetention": {
          "target": "com.amazonaws.s3#BypassGovernanceRetention",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-bypass-governance-retention"
            },
            "smithy.api#httpHeader": "x-amz-bypass-governance-retention"
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
              "$ref": "#header-x-amz-sdk-checksum-algorithm"
            },
            "smithy.api#httpHeader": "x-amz-sdk-checksum-algorithm"
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
