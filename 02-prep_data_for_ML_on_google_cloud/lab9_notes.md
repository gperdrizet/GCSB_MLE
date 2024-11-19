# Lab 9: Video Intelligence: Qwik Start

Extracting metadata and annotation from video.

## Objectives

- Set up authorization for a custom service account
- Send an annotate video request to the Video Intelligence API

## 1. Set up authorization

From Cloud Shell, create a service account:

```bash
gcloud iam service-accounts create quickstart
```

Then create a service account key file for the project:

```bash
gcloud iam service-accounts keys create key.json --iam-account quickstart@qwiklabs-gcp-01-c9fd3b89fe23.iam.gserviceaccount.com
```

Log the service account in:

```bash
gcloud auth activate-service-account --key-file key.json
```

Get authorization token:

```bash
$ gcloud auth print-access-token

ya29.c.c0ASRK0GY7cf0REId_kzUgRnfS7UIfASZtM2Mtnq0ktHGY3U3ly1vfa071lTD1_XRfXEa9NqN6MZ4QlFxQ6kOyOexzFbAQM0bt9RYFtpqfrMqUB8eYvSz5ph8_8qxZ6nnsnE8_-j3mmLk1dfPuEeQpUxrIFshP0a8sdSxAZ9KSQfA1JN5CaygVmz0ak5Qr7rOH0BEAyicMQULarG73HQXEkqtvEg_SgvjYjoamm998gllnjqOAgumhVdpJI0xi79EQnluIOAOUIHxioochDUzyBGnHJSzExvXViRG0MOGYBmtpH_SmoWDY9eAuCF3E_N0HMsakiAXsgd4nWAXlHwwzQMZ4xYLfIA7PrSpIL6pHUYZMKNwQu9H9O8qh1yB0pCgVA5vET397Pe9s4dgky2OW97gmZWj8prfwUX3w6zB2ndhhqXx2k7eloI73pZ4a7uQi2OsIt71VcwJw7cXBiZ_eray5BQ1oj9sia_g3Zs5bo54v_aIz3jRQsS36Vk9lMSmmSpUOggXIBzOW0yVIbbvg9UjfjUfrhSZ280kmx_dvrQnXUu1mWypIea2Qm_MFoxyxh7Vyklds2w0Vj2ysuXi0RJ989uaehdxVwrIm_V4nxZc5Mhn0JMJp9FcSadyV3vJgMjiq785ckB-QiSwy_J1BF_FtQnlkwd7gVwnMqZWimVpsSQMmIpk1MFwQ-cfqsymXpY8gZiS65xJjmiyFWf4ZcIdOiwh2lgbSMnakBmxUuurnR4WMoi1FpWWRV4wUQ82BQ4vZFidp4_Yb8aIwIabmMxkRUx9nz1ryez8Y86RJgZuUZgpX11Q400UsUvxytjwJM-qcReVsqz-lMY4p9J6p6uqq0fROx50sOWk3Q06ZpJ5zZWmhJpn7Oum__XuUYpx54iw5JWjZ39r0mbqn5Yqxl_Jczp02aV4jSeUti3wd9nzy_-d_8MjsV4ihq9YupqcZ8bssumB2ddWvOobW36pUiaFJsRXZjitujbmuI9mM1rFhOzc8Oqxeqsb
```

## 2. Make an annotate video request

Create the request:

```bash
cat > request.json <<EOF
{
   "inputUri":"gs://spls/gsp154/video/train.mp4",
   "features": [
       "LABEL_DETECTION"
   ]
}
EOF
```

Send the request:

```bash
$ curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/videos:annotate' \
    -d @request.json

{
  "name": "projects/39852241774/locations/us-east1/operations/15056824783883721419"
}
```

Check the status of the opperation:

```bash
$ curl -s -H 'Content-Type: application/json' \
    -H 'Authorization: Bearer '$(gcloud auth print-access-token)'' \
    'https://videointelligence.googleapis.com/v1/projects/39852241774/locations/us-east1/operations/15056824783883721419'

{
  "name": "projects/39852241774/locations/us-east1/operations/15056824783883721419",
  "metadata": {
    "@type": "type.googleapis.com/google.cloud.videointelligence.v1.AnnotateVideoProgress",
    "annotationProgress": [
      {
        "inputUri": "/spls/gsp154/video/train.mp4",
        "progressPercent": 100,
        "startTime": "2024-11-19T17:04:18.364623Z",
        "updateTime": "2024-11-19T17:04:31.293387Z"
      }
    ]
  },
  "done": true,
  "response": {
    "@type": "type.googleapis.com/google.cloud.videointelligence.v1.AnnotateVideoResponse",
    "annotationResults": [
      {
        "inputUri": "/spls/gsp154/video/train.mp4",
        "segmentLabelAnnotations": [
          {
            "entity": {
              "entityId": "/m/01vk9q",
              "description": "track",
              "languageCode": "en-US"
            },
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9854606
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/025s53m",
              "description": "rolling stock",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.3284738
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0467y7",
              "description": "passenger car",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.782828
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/07yv9",
              "description": "vehicle",
              "languageCode": "en-US"
            },
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9194523
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/01prls",
              "description": "land vehicle",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9941471
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/04h5c",
              "description": "locomotive",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.98347765
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/01g50p",
              "description": "railroad car",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9871346
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0195fx",
              "description": "rapid transit",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07bsy",
                "description": "transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.8040178
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0db2f",
              "description": "high speed rail",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/06d_3",
                "description": "rail transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.32582685
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/07jdr",
              "description": "train",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.99541986
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/05zdp",
              "description": "public transport",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07bsy",
                "description": "transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9222028
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/06d_3",
              "description": "rail transport",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07bsy",
                "description": "transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9922013
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0py27",
              "description": "train station",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/0cgh4",
                "description": "building",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.7776639
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/07bsy",
              "description": "transport",
              "languageCode": "en-US"
            },
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.98366255
              }
            ]
          }
        ],
        "shotLabelAnnotations": [
          {
            "entity": {
              "entityId": "/m/01vk9q",
              "description": "track",
              "languageCode": "en-US"
            },
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9854606
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/025s53m",
              "description": "rolling stock",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.31272778
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0467y7",
              "description": "passenger car",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.782828
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/05zdp",
              "description": "public transport",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07bsy",
                "description": "transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.92998904
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/01prls",
              "description": "land vehicle",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9941471
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/04h5c",
              "description": "locomotive",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.98347765
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/01g50p",
              "description": "railroad car",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.98800284
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0195fx",
              "description": "rapid transit",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07bsy",
                "description": "transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.811784
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/06d_3",
              "description": "rail transport",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07bsy",
                "description": "transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.9922013
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/07bsy",
              "description": "transport",
              "languageCode": "en-US"
            },
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.98366255
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/07jdr",
              "description": "train",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/07yv9",
                "description": "vehicle",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.99541986
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/07yv9",
              "description": "vehicle",
              "languageCode": "en-US"
            },
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.92183924
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0db2f",
              "description": "high speed rail",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/06d_3",
                "description": "rail transport",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.32582685
              }
            ]
          },
          {
            "entity": {
              "entityId": "/m/0py27",
              "description": "train station",
              "languageCode": "en-US"
            },
            "categoryEntities": [
              {
                "entityId": "/m/0cgh4",
                "description": "building",
                "languageCode": "en-US"
              }
            ],
            "segments": [
              {
                "segment": {
                  "startTimeOffset": "0s",
                  "endTimeOffset": "12.640s"
                },
                "confidence": 0.7776639
              }
            ]
          }
        ],
        "segment": {
          "startTimeOffset": "0s",
          "endTimeOffset": "12.640s"
        }
      }
    ]
  }
}
```
