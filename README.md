# GuideEye 👁️

**Real-time AI vision and navigation assistant for people who are blind or have low vision.**
Built by Team 34 (*Command Not Found*) at the **AWS x INRIX Hackathon**.

GuideEye turns a phone or laptop camera into a talking guide: it watches the scene, warns about obstacles and pedestrians, reads signs aloud, and helps the user get where they're going, including routing to the nearest hospital in an emergency.

## ✨ Features

- **Live hazard warnings:** detects objects, people and vehicles in camera frames and speaks short warnings only when the scene meaningfully changes, so the user isn't flooded with audio.
- **On-demand narration ("Tell me now"):** one tap for a natural-language description of the surroundings, with an optional *Detailed AI* mode for richer descriptions.
- **Text reading:** reads street signs, labels and other visible text aloud.
- **Navigation:** geocoding, nearby-place search and walking directions from the user's live location.
- **Emergency routing:** when an alert is raised, finds the nearest hospital and returns a route to it.
- **Voice in and out:** speech-to-text for commands and text-to-speech for responses.
- **Photo, video and live camera modes** in the web UI.

## 🏗️ Architecture

```
Browser (index.html)
  ├─ camera frames / uploads ──► API Gateway ──► Lambda: vision + navigation
  │                                               ├─ Amazon Rekognition (labels, bounding boxes, text)
  │                                               ├─ Amazon Bedrock (Claude) for scene descriptions
  │                                               └─ Google Maps APIs (geocode, places, directions)
  └─ voice ─────────────────────► Lambda: VoiceProcessing
                                                  ├─ Amazon Polly (text-to-speech)
                                                  └─ Amazon Transcribe + S3 (speech-to-text)
```

Infrastructure is managed with **AWS Amplify** (`amplify/`), which provisions the REST API and both Python Lambda functions.

## 🛠️ Tech Stack

| Layer | Tools |
|---|---|
| Frontend | HTML, CSS, JavaScript, `getUserMedia`, Web Speech API |
| Backend | Python 3, AWS Lambda, API Gateway |
| AI / ML | Amazon Rekognition, Amazon Bedrock (Claude Sonnet / Haiku) |
| Voice | Amazon Polly, Amazon Transcribe |
| Maps | Google Maps Geocoding, Places and Directions APIs |
| Infra | AWS Amplify, CloudFormation, S3 |

## 🚀 Getting Started

**Prerequisites:** an AWS account with Rekognition, Bedrock (model access enabled), Polly and Transcribe; the Amplify CLI; a Google Maps API key.

```bash
git clone https://github.com/bganapuram-spec/GuideEye.git
cd GuideEye

amplify init        # or: amplify pull, to attach to an existing backend
amplify push        # deploys the API and Lambda functions
```

1. Set `GOOGLE_MAPS_API_KEY` as an environment variable on the vision Lambda.
2. Update `S3_BUCKET` in `amplify/backend/function/VoiceProcessing/src/index.py` to your bucket.
3. Point the API endpoint in `index.html` at your deployed API, then open `index.html` in a browser (camera access requires HTTPS or `localhost`).

## 📁 Project Structure

```
GuideEye/
├── index.html                 # Web UI: camera, uploads, narration, voice
├── amplify.yaml               # Amplify build settings
└── amplify/backend/
    ├── api/                   # REST API definition
    └── function/
        ├── awsxinrixhackathon…/src/index.py   # Vision, scene description, maps
        └── VoiceProcessing/src/index.py       # Polly TTS + Transcribe STT
```

## 👥 Team

Forked from the original hackathon repo [shiremath2-hir/Command-not-found-34](https://github.com/shiremath2-hir/Command-not-found-34).
