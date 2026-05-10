# Google Cloud Vision

Image labelling, face detection, and visual search using Google Cloud Vision API — running entirely in Colab.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/wsamuelw/google-cloud-vision/blob/master/google_cloud_vision.ipynb)

## Problem

Computer vision models are powerful but require significant infrastructure to deploy. Google Cloud Vision API makes this accessible — send an image, get labels, face analysis, and visual search results. This project demonstrates three core capabilities: web detection, face detection, and similar image search.

## What's Inside

| Capability | What It Does | API Method |
|-----------|-------------|------------|
| **Web Detection** | Labels the image, finds entities, returns similar images | `web_detection()` |
| **Face Detection** | Detects emotions (joy, sorrow, anger, surprise), blur, exposure, headwear | `face_detection()` |
| **Visual Search** | Finds visually similar images across the web | `visually_similar_images` |

## Pipeline

```
Image → Authentication → Vision API Request → Parse Response → Display Results
```

| Step | What Happens |
|------|-------------|
| Auth | Load Google Cloud service account key |
| Client | Create `ImageAnnotatorClient` |
| Web detect | Best guess labels + entity scores + similar images |
| Face detect | Emotion likelihoods (joy, sorrow, anger, surprise) + technical attributes |

## Results

### Web Detection — Best Guess Label

The API identifies the subject of the image with a confidence score. It also returns:
- **Web entities** — related concepts detected in the image
- **Visually similar images** — other images across the web that look like this one

### Face Detection — Emotion Analysis

| Attribute | What It Measures |
|-----------|-----------------|
| Joy likelihood | How likely the face is happy |
| Sorrow likelihood | How likely the face is sad |
| Anger likelihood | How likely the face is angry |
| Surprise likelihood | How likely the face is surprised |
| Under exposed | Image quality — is the face too dark? |
| Blurred | Image quality — is the face out of focus? |
| Headwear | Is the person wearing headwear? |

## Setup

### Google Colab

Click the badge above. You'll need a Google Cloud service account key:

1. Go to [Google Cloud Console → Credentials](https://console.cloud.google.com/apis/credentials/serviceaccountkey)
2. Create a service account key (JSON)
3. Upload the JSON file to Colab
4. Set the filename in `os.environ["GOOGLE_APPLICATION_CREDENTIALS"]`

### Local

```bash
pip install google-cloud-vision matplotlib
git clone https://github.com/wsamuelw/google-cloud-vision.git
cd google-cloud-vision
jupyter notebook google_cloud_vision.ipynb
```

## Key Code

### Web Detection

```python
client = vision.ImageAnnotatorClient()
response = client.web_detection(image=input_image)

# best guess label
print(response.web_detection.best_guess_labels)

# entity scores
for entity in response.web_detection.web_entities:
    print(entity.description, entity.score)

# visually similar images
print(response.web_detection.visually_similar_images[:5])
```

### Face Detection

```python
response = client.face_detection(image=input_image)
for face in response.face_annotations:
    print(f'Joy: {face.joy_likelihood}')
    print(f'Anger: {face.anger_likelihood}')
```

## Tech Stack

- **Google Cloud Vision API** — image analysis (web detection, face detection)
- **matplotlib** — image display
- **google-cloud-vision** — Python client

## Use Cases

- **Content moderation** — detect inappropriate content at scale
- **E-commerce** — visual search for product matching
- **Accessibility** — auto-generate alt text for images
- **Sentiment analysis** — detect emotions in photos
- **Brand monitoring** — find where your images appear online

## References

- [Google Cloud Vision API docs](https://cloud.google.com/vision/docs)
- [Python client reference](https://cloud.google.com/python/docs/reference/vision/latest)

## License

MIT
