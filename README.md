# News Topic Public Opinion Mining Agent

An AI-powered public opinion mining system that takes a **news topic or headline** as input, collects relevant YouTube comments, and analyzes how people respond to the topic through **named entities, emotions, stance, discussion aspects, clustering, summaries, and visual analytics**.

The project is implemented as an interactive **Google Colab** application and combines traditional NLP techniques with Gemini AI.

---

## Overview

The system follows this pipeline:

```text
News Topic / Headline
        │
        ▼
   YouTube Search
        │
        ▼
 Relevant Videos
        │
        ▼
  YouTube Comments
        │
        ▼
 Preprocessing & Filtering
        │
        ├──────────────► Named Entity Recognition
        │                    │
        │                    ▼
        │               Key Entities
        │
        ├──────────────► Emotion Detection
        │                    │
        │                    ▼
        │            Joy / Anger / Fear /
        │         Sadness / Disgust / Surprise
        │
        ├──────────────► Stance Detection
        │                    │
        │                    ▼
        │          Support / Oppose / Neutral
        │
        └──────────────► TF-IDF + K-Means
                             │
                             ▼
                       Discussion Aspects

                         ↓
                    Gemini AI
                         ↓
          ┌──────────────┼──────────────┐
          ▼              ▼              ▼
      Summaries      Aspect Analysis   Infographic
```

---

## Key Features

### 1. YouTube Comment Collection

The system searches YouTube for videos related to the supplied news topic and collects comments from relevant videos.

Current configuration:

- Up to **5 videos**
- Up to **1,500 raw comments**
- Maximum **300 comments per video**
- Minimum video-view and comment thresholds

The project uses the **YouTube Data API v3**.

---

### 2. Comment Preprocessing

Collected comments are processed before analysis.

The preprocessing pipeline includes:

- English-language filtering
- Hinglish/noise filtering
- URL and mention removal
- Special-character removal
- Duplicate comment removal
- Random sampling for efficient processing

---

### 3. Named Entity Recognition

**spaCy NER** is used to identify important entities appearing in public discussions.

The current implementation focuses on:

```text
PERSON
ORG
GPE
```

The most frequently mentioned entities are selected for deeper analysis.

---

### 4. Emotion Detection

Emotion analysis is performed using **NRCLex**.

The system maps detected emotions into:

```text
Joy
Anger
Sadness
Fear
Disgust
Surprise
Neutral
```

Both overall emotion distribution and entity-level emotion distribution are generated.

---

### 5. Stance Detection

The system classifies comments into:

```text
Support
Oppose
Neutral
```

The current implementation uses a keyword-based scoring mechanism with separate support and opposition vocabularies.

For each comment:

```text
Support score > Oppose score → Support

Oppose score > Support score → Oppose

Otherwise → Neutral
```

This produces both overall and entity-specific stance distributions.

---

### 6. Discussion Aspect Extraction

Comments related to major entities are clustered using:

```text
TF-IDF
     +
K-Means
```

Important terms from each cluster are extracted to represent the main aspects or sub-topics being discussed.

Example:

```text
Entity: Example Organization

Aspects:
- funding / budget / cost
- development / project / progress
- policy / government / decision
```

---

### 7. Summarization

The project uses Gemini AI to generate:

- Overall public-opinion summary
- Stance summary
- Entity-level summaries
- Aspect-level summaries

A **TF-IDF/TextRank-based fallback** is also included when Gemini generation is unavailable.

---

### 8. AI-Generated Analytics Image

Gemini image generation is used to create an infographic summarizing the analysis.

The generated visualization includes information such as:

- Key entities
- Dominant emotions
- Support / Oppose / Neutral distribution
- Entity-level emotional information

---

## Dashboard

The project generates a visual analytics dashboard containing:

- Overall emotion distribution
- Overall stance distribution
- Entity mention frequency
- Entity × emotion analysis
- Entity × stance analysis
- Interpretation panel

Generated outputs are saved in the `output/` directory.

---

## Technology Stack

### Programming

- Python
- Google Colab

### NLP / Machine Learning

- spaCy
- NRCLex
- scikit-learn
- TF-IDF
- K-Means
- TextRank-style summarization

### Data Collection

- YouTube Data API v3
- `google-api-python-client`

### Generative AI

- Google Gemini
- Gemini text generation
- Gemini image generation

### Visualization

- Matplotlib
- IPyWidgets
- Pillow

---

## Project Structure

```text
news-topic-public-opinion-mining-agent/
│
├── opinion_mining_agent.ipynb
├── README.md
├── requirements.txt
├── .gitignore
│
└── output/
    └── .gitkeep
```

---

## Installation

Install the required Python packages:

```bash
pip install -q google-api-python-client langdetect \
    ipywidgets matplotlib nrclex psutil \
    google-generativeai google-genai Pillow \
    spacy scikit-learn numpy
```

Download the spaCy English model:

```bash
python -m spacy download en_core_web_sm
```

---

## API Keys

The project requires:

- YouTube Data API v3 key
- Google Gemini API key

**Do not hard-code API keys in the notebook or commit them to GitHub.**

For Google Colab, store the keys in **Colab Secrets** and retrieve them using:

```python
from google.colab import userdata

YOUTUBE_API_KEY = userdata.get("YOUTUBE_API_KEY")
GEMINI_API_KEY = userdata.get("GEMINI_API_KEY")
```

---

## Running the Project

Open:

```text
opinion_mining_agent.ipynb
```

in Google Colab.

Enter a news topic such as:

```text
NASA Artemis lunar landing
```

or:

```text
India Budget
```

Then click:

```text
Analyse
```

The system collects comments and generates the complete opinion analysis.

---

## Example Output

The system produces:

```text
1. Top discussed entities
2. Overall emotion distribution
3. Overall stance distribution
4. Entity-level emotion analysis
5. Entity-level stance analysis
6. Discussion aspects
7. Gemini-generated summaries
8. Opinion mining dashboard
9. AI-generated analytics infographic
10. Text report
```

---

## Research Motivation

Online discussions contain large amounts of user-generated opinion about current events.

This project explores how a combination of:

```text
Information Extraction
+
Emotion Analysis
+
Stance Analysis
+
Topic/Aspect Discovery
+
Generative AI
```

can be used to transform raw social-media discussion into structured public-opinion insights.

---

## Limitations

The current prototype has several limitations:

- Data collection depends on the availability and accessibility of YouTube comments.
- Emotion detection uses a lexicon-based approach.
- Stance classification is currently keyword-based rather than transformer-based.
- Aspect discovery is based on TF-IDF and clustering.
- Results can contain noise, sarcasm, slang, or context-dependent interpretations.
- Gemini-generated summaries depend on the quality of the analyzed comments.

These limitations provide opportunities for future improvements using transformer-based emotion/stance models and more advanced aspect-based sentiment analysis.

---

## Future Improvements

- Transformer-based stance classification
- Aspect-Based Sentiment Analysis (ABSA)
- Better sarcasm and multilingual-language detection
- Larger-scale comment collection
- Temporal opinion analysis
- Topic evolution tracking
- Reddit and other public-platform integration
- Automatic comparison of public opinion across platforms

---

## Author

**Mohit Tomar**  
M.Tech — Computer Science and Information Security  
NIT Warangal

---

## Disclaimer

This project is intended for academic and research experimentation. The generated opinion analysis represents patterns in the collected comments and should not be interpreted as a statistically representative survey of the entire population.
