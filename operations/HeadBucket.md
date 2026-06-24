# HeadBucket

[Request](#request) → [Response](#response-200-success) 𝄁 [Error](#errors)

Determines if a bucket exists and if the requester is authorized to access it.

**Metadata Probe:** The operation returns an HTTP `200 OK` status code if the target bucket exists and the client credentials are authorized to perform the action. If the bucket does not exist or access is denied, the storage engine returns standard HTTP status codes (such as `404 Not Found` or `403 Forbidden`). Because this is a `HEAD` request, no response body is returned with error details.

**Permissions:** Requesters must have permission to perform metadata checks or list the bucket. Under the standard S3 IAM policy model, this requires the `s3:ListBucket` action. The bucket owner has this permission by default.

## Request

```HTTP
HEAD /{bucket} HTTP/1.1
```

### Path: `bucket`

The name of the bucket to check.

### Header: `x-amz-expected-bucket-owner`

The account identifier of the expected bucket owner. If the actual owner does not match, the request fails with an HTTP `403 Forbidden` error.

## Response: `200` Success

```HTTP
HTTP/1.1 200 OK

```

### Response Header: `x-amz-bucket-arn`

The Amazon Resource Name (ARN) identifying the bucket, if returned by the storage engine.

### Response Header: `x-amz-bucket-location-type`

The location type classification of the bucket (e.g., Regional).

### Response Header: `x-amz-bucket-location-name`

The specific name of the location or zone where the bucket is deployed.

### Response Header: `x-amz-bucket-region`

The location region of the bucket.

### Response Header: `x-amz-access-point-alias`

Indicates whether the bucket name used in the request corresponds to an access point alias.

## Errors

### `404` `NotFound`

The specified bucket does not exist.

## Smithy Spec

<details>

```json
{
  "smithy": "2.0",
  "shapes": {
    "com.amazonaws.s3#HeadBucket": {
      "type": "operation",
      "input": {
        "target": "com.amazonaws.s3#HeadBucketRequest"
      },
      "output": {
        "target": "com.amazonaws.s3#HeadBucketOutput"
      },
      "errors": [
        {
          "target": "com.amazonaws.s3#NotFound"
        }
      ],
      "traits": {
        "smithy.api#documentation": {
          "$ref": "#headbucket"
        },
        "smithy.api#examples": [
          {
            "title": "To determine if bucket exists",
            "documentation": "This operation checks to see if a bucket exists.",
            "input": {
              "Bucket": "acl1"
            }
          }
        ],
        "smithy.api#http": {
          "method": "HEAD",
          "uri": "/{Bucket}",
          "code": 200
        },
        "smithy.waiters#waitable": {
          "BucketExists": {
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
          "BucketNotExists": {
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
    "com.amazonaws.s3#HeadBucketOutput": {
      "type": "structure",
      "members": {
        "BucketArn": {
          "target": "com.amazonaws.s3#S3RegionalOrS3ExpressBucketArnString",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-bucket-arn"
            },
            "smithy.api#httpHeader": "x-amz-bucket-arn"
          }
        },
        "BucketLocationType": {
          "target": "com.amazonaws.s3#LocationType",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-bucket-location-type"
            },
            "smithy.api#httpHeader": "x-amz-bucket-location-type"
          }
        },
        "BucketLocationName": {
          "target": "com.amazonaws.s3#BucketLocationName",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-bucket-location-name"
            },
            "smithy.api#httpHeader": "x-amz-bucket-location-name"
          }
        },
        "BucketRegion": {
          "target": "com.amazonaws.s3#Region",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-bucket-region"
            },
            "smithy.api#httpHeader": "x-amz-bucket-region"
          }
        },
        "AccessPointAlias": {
          "target": "com.amazonaws.s3#AccessPointAlias",
          "traits": {
            "smithy.api#documentation": {
              "$ref": "#response-header-x-amz-access-point-alias"
            },
            "smithy.api#httpHeader": "x-amz-access-point-alias"
          }
        }
      },
      "traits": {
        "smithy.api#output": {}
      }
    },
    "com.amazonaws.s3#HeadBucketRequest": {
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
