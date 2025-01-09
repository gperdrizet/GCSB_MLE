# Lab 7: Cloud Natural Language API: Qwik Start

Can do : entity recognition, sentiment analysis, information extraction, question answering via REST API.

## Objectives

- Create an API key
- Use the Cloud Natural Language API to extract "entities" (e.g. people, places, and events) from a snippet of text

## 1. Create an API key

Via cloud console, set project ID as environment variable:

```bash
export GOOGLE_CLOUD_PROJECT=$(gcloud config get-value core/project)
```

Create service account for API access:

```bash
gcloud iam service-accounts create my-natlang-sa \
  --display-name "my natural language service account"
```

Create and save credentials to `key.json`:

```bash
gcloud iam service-accounts keys create ~/key.json \
  --iam-account my-natlang-sa@${GOOGLE_CLOUD_PROJECT}.iam.gserviceaccount.com
```

Set credentials via environment variable:

```bash
export GOOGLE_APPLICATION_CREDENTIALS="/home/USER/key.json"
```

## 2. Make entity analysis request

Connect to Compute Engine linux instance via ssh:

- Navigation menu > Compute Engine
- linux instance > SSH

Send the request:

```bash
$ gcloud ml language analyze-entities --content="Michelangelo Caravaggio, Italian painter, is known for 'The Calling of Saint Matthew'." > result.json
$ cat result.json

{
  "entities": [
    {
      "mentions": [
        {
          "text": {
            "beginOffset": 0,
            "content": "Michelangelo Caravaggio"
          },
          "type": "PROPER"
        },
        {
          "text": {
            "beginOffset": 33,
            "content": "painter"
          },
          "type": "COMMON"
        }
      ],
      "metadata": {
        "mid": "/m/020bg",
        "wikipedia_url": "https://en.wikipedia.org/wiki/Caravaggio"
      },
      "name": "Michelangelo Caravaggio",
      "salience": 0.82904786,
      "type": "PERSON"
    },
    {
      "mentions": [
        {
          "text": {
            "beginOffset": 25,
            "content": "Italian"
          },
          "type": "PROPER"
        }
      ],
      "metadata": {},
      "name": "Italian",
      "salience": 0.13981608,
      "type": "LOCATION"
    },
    {
      "mentions": [
        {
          "text": {
            "beginOffset": 56,
            "content": "The Calling of Saint Matthew"
          },
          "type": "PROPER"
        }
      ],
      "metadata": {
        "mid": "/m/085_p7",
        "wikipedia_url": "https://en.wikipedia.org/wiki/The_Calling_of_Saint_Matthew"
      },
      "name": "The Calling of Saint Matthew",
      "salience": 0.031136045,
      "type": "EVENT"
    }
  ],
  "language": "en"
}
```
