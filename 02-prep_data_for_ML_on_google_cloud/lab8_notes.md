# Lab 8: Speech-to-Text API: Qwik Start

## Objectives

- Create an API key
- Create a Speech-to-Text API request
- Call the Speech-to-Text API

## 1. Create an API key

- Navigation menu > APIs & services > Credentials > Create credentials
- API key > copy the key > close

**key**: AIzaSyBHkHDhTnkqJTM1zs8y8sM7ZOMjmT37_3Y

Set key as environment variable in Compute Engine linux instance:

- Navigation menu > Compute Engine > linux instance > SSH

```bash
export API_KEY=AIzaSyBHkHDhTnkqJTM1zs8y8sM7ZOMjmT37_3Y
```

## 2. Create your speech-to-text API request

Create and add the following to `./request.json`:

```text
{
  "config": {
      "encoding":"FLAC",
      "languageCode": "en-US"
  },
  "audio": {
      "uri":"gs://cloud-samples-tests/speech/brooklyn.flac"
  }
}
```

## 3. Call the speech-to-text API

```bash
$ curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}"

{
  "results": [
    {
      "alternatives": [
        {
          "transcript": "how old is the Brooklyn Bridge",
          "confidence": 0.93075204
        }
      ],
      "resultEndTime": "1.770s",
      "languageCode": "en-us"
    }
  ],
  "totalBilledTime": "2s",
  "requestId": "1109139467200899502"
}
```

Save the response:

```bash
curl -s -X POST -H "Content-Type: application/json" --data-binary @request.json \
"https://speech.googleapis.com/v1/speech:recognize?key=${API_KEY}" > result.json
```
