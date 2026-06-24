# DeleteObject

[Request](#request) → [Response](#response-204-success)

Removes an object from a bucket.

**Versioning Semantics:** The behavior of this operation depends on the bucket's versioning state:
- **Versioning Disabled:** The object is permanently and immediately deleted.
- **Versioning Enabled:** The storage engine creates a new, zero-byte "delete marker" and makes it the current version. The original version remains in the bucket's history. To permanently delete a specific version (or a delete marker), specify its version identifier in the `versionId` query parameter.
- **Versioning Suspended:** The storage engine removes the object version that has a `null` version ID (if it exists) and inserts a delete marker as the current version.

**Permissions:** Requesters must have permission to perform delete operations on the target bucket and object key path. Under the standard S3 IAM policy model, this requires the `s3:DeleteObject` action. Deleting a specific object version requires the `s3:DeleteObjectVersion` action.

## Request

```HTTP
DELETE /{bucket}/{key} HTTP/1.1
```

### Path: `bucket`

The name of the bucket containing the object to delete.

### Path: `key`

The object key to delete.

### Query: `versionId`

Specifies the version identifier of the object to permanently delete. If not specified, the delete operation applies to the current version.

### Header: `x-amz-mfa`

> **Avoid:** MFA delete configurations are legacy and unsupported by many engines.

### Header: `x-amz-request-payer`

Confirms that the requester understands they will be charged for processing the request. This header is required when accessing buckets configured with requester-pays billing enabled.

### Header: `x-amz-bypass-governance-retention`

Specifies whether to bypass Object Lock Governance-mode retention configurations to delete the object version.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

### Header: `if-match`

Deletes the object only if its entity tag (ETag) matches the provided value. If the values do not match, the operation returns a `412 Precondition Failed` error.

### Header: `x-amz-if-match-last-modified-time`

> **Note:** Likely not portable. May be removed.

Deletes the object only if its last modified timestamp matches the specified value.

### Header: `x-amz-if-match-size`

> **Note:** Likely not portable. May be removed.

Deletes the object only if its size matches the specified value in bytes.

## Response: `204` Success

```HTTP
HTTP/1.1 204 No Content

```

### Response Header: `x-amz-delete-marker`

Indicates whether the deletion resulted in a new delete marker (true), or if the deleted version was itself a delete marker.

### Response Header: `x-amz-version-id`

Returns the version identifier of the newly created delete marker, if applicable.

### Response Header: `x-amz-request-charged`

Indicates if the request was charged under requester-pays billing rules.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#DeleteObject": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#DeleteObjectRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#DeleteObjectOutput"
      },
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#deleteobject"
        },
        "smithy.api#examples": [
          {
            "title": "To delete an object (from a non-versioned bucket)",
            "documentation": "The following example deletes an object from a non-versioned bucket.",
            "input": {
              "Bucket": "ExampleBucket",
              "Key": "HappyFace.jpg"
            }
          },
          {
            "title": "To delete an object",
            "documentation": "The following example deletes an object from an S3 bucket.",
            "input": {
              "Bucket": "examplebucket",
              "Key": "objectkey.jpg"
            },
            "output": {}
          }
        ],
        "smithy.api#http": {
          "method": "DELETE",
          "uri": "/{Bucket}/{Key+}?x-id=DeleteObject",
          "code": 204
        }
      }
    },
    "com.amazonaws.s3#DeleteObjectOutput": {
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
        "VersionId": {
          "target": "com.amazonaws.s3#ObjectVersionId",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-version-id"
            },
            "smithy.api#httpHeader": "x-amz-version-id"
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
    "com.amazonaws.s3#DeleteObjectRequest": {
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
        "MFA": {
          "target": "com.amazonaws.s3#MFA",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-mfa"
            },
            "smithy.api#httpHeader": "x-amz-mfa"
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
        "IfMatch": {
          "target": "com.amazonaws.s3#IfMatch",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-if-match"
            },
            "smithy.api#httpHeader": "If-Match"
          }
        },
        "IfMatchLastModifiedTime": {
          "target": "com.amazonaws.s3#IfMatchLastModifiedTime",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-if-match-last-modified-time"
            },
            "smithy.api#httpHeader": "x-amz-if-match-last-modified-time"
          }
        },
        "IfMatchSize": {
          "target": "com.amazonaws.s3#IfMatchSize",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#header-x-amz-if-match-size"
            },
            "smithy.api#httpHeader": "x-amz-if-match-size"
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
