# WholesomeGreentext: Fine-tuning Language Models for Style Transfer

I built this project as a hands-on educational project about idiosyncratic style transfer of language models and presented the results at AI Tinkerers Munich to about a hundred people. 

This was a deliberate experiment in working with language models whilst not running into cloud payment induced bankruptcy. The training was done completely on Kaggle and Google Colab free tiers.

## Project Overview

I wanted to work with language models, but because of finite financial resources, I was constrained to small models that would fit into the RAM of freely available GPUs. This led me to build on top of the work of [TinyStories](https://arxiv.org/abs/2305.07759), who demonstrated that even LMs with just 33M parameters could produce coherent stories when trained on a limited vocabulary corpus of synthetic children's stories.

I wanted to see if these small yet capable models could perform style transfer while maintaining coherent outputs. To test this, I chose to focus on reproducing the unique linguistic style of 4chan greentext stories. I constrained myself to wholesome greentext scraped from the corresponding `r/wholesomegreentext` subreddit variety to reduce toxicity (though further post-processing was still necessary).

Key project areas:
- Data collection and preprocessing from Reddit
- Synthetic data generation and curation
- Fine-tuning of lightweight transformer models
- Model deployment and web application development

An example of the target style (thank you genre):
```text
>be you, reading this README
>ask yourself, what is a thank you genre greentext example?
>this
>very meta
>thanks README.md
```

## Repository Structure
```
├── README.md                  # Main project documentation
├── requirements.txt           # Project dependencies
├── data/
│   ├── collected/             # Real data collection pipeline
│   │   ├── QA_app/            # Data curation application
│   │   │   ├── README.md      # Setup instructions for QA app
│   │   │   ├── flask_backend/ # Backend server
│   │   │   └── frontend/      # React frontend
│   │   ├── images/            # Scraped greentext images
│   │   ├── reddit_wholesomegreentext_posts/  # Raw Reddit data sample (full data on Huggingface)
│   │   └── *.ipynb            # Data collection notebooks
│   └── synthetic/             # Synthetic data generation
│       ├── output/            # Generated data sample (full data on Huggingface)
│       └── *.ipynb            # Generation notebooks
└── training/                  # Model training notebooks
    └── finetune_tinystories.ipynb
```

## Technical Implementation

### Data Pipeline

The project started with real data collection from `r/wholesomegreentext`. Since greentexts are mostly submitted as screenshots, I built a pipeline using Reddit's Pushshift API for scraping and Tesseract for OCR. I quickly realized I needed a way to validate and clean the extracted text, so I built a small QA app (you'll find it in `data/collected/QA_app`).

However, it soon became apparent that the real-world corpus wasn't large enough for high-quality fine-tuning. This led to me experimenting with synthetic data generation. 

You find the synthetic generation code in `data/synthetic/openAI_api.ipynb`.

I found that I could get satisfactory style transfer once I had sufficient variety in synthetic examples while carefully controlling vocabulary to match the 33M model's capabilities. Restricting the fine-tuning to specific subgenres (like the "thank you" style) further improved results on this particular style.

1. **Real Data Collection**
   - Reddit screenshot scraping via Pushshift API
   - OCR text extraction using Pytesseract
   - Manual data curation through custom QA application

2. **Synthetic Data Generation**
   - OpenAI API for data generation
   - Specialized datasets:
     - Standard 4chan-style greentexts
     - TinyStories-compatible vocabulary greentexts
     - Genre-specific "thank you" greentexts
   - Total dataset: 239k examples
   - Available on [Huggingface](https://huggingface.co/maxmyn)

### Model Development

Training was mostly done in Jupyter Notebooks on the free tiers of Kaggle and Google Colab. Rather than developing quantitative metrics, I "vibe checked" the model results, focusing on qualitative assessment of style transfer and coherence.

Example training code is in the `training\finetune_tinystories.ipynb` notebook, which was run on Google Colab or Kaggle.

1. **TinyStories-based Models**
   - Fine-tuned 33M parameter model
   - Focused on simplified vocabulary
   - Best results with genre-specific training

2. **DistilGPT-2-based Models**
   - Custom token sequence training
   - '>' token often prompted code generation. I could mitigate this by training a specific `<|4changreentext|>` token.

### Key Findings
- Data quality and specificity outweighed quantity
- Synthetic data generation proved surprisingly steerable and was the biggest lever on model behavior
- Genre-specific training showed superior performance despite smaller dataset size 
- Base model vocabulary significantly impacted results

### Infrastructure
- Frontend: React (Railway deployment)
- Model Hosting: Huggingface
- Training: Kaggle and Google Colab free tiers
- Data Storage: Huggingface Datasets

Previously, you could play with the models on wholesomegreentexts.lol, but I haven't renewed the URL. However, you can still find all the models and datasets on [Huggingface](https://huggingface.co/maxmyn).

## Development Setup

1. **Data Curation App**
- A prototype-quality tool for rapid data curation
- Includes sample data: 2 images in `data/collected/images/` with transcriptions in `db.json`
- Note: Backend must be run from `flask_backend` directory due to hardcoded paths
- See QA_app/README.md for more information

2. **Training Environment**
- Notebooks compatible with Kaggle/Colab
- See individual notebook requirements

## Technical Challenges
1. **Dataset Size Limitations**
   - The initial real-world dataset was too small for good results
   - Solved by developing a synthetic data generation pipeline using OpenAI's API

2. **Vocabulary Control**
   - Had to carefully manage text complexity while maintaining style
   - Created filtered datasets matching TinyStories vocabulary

3. **Token Disambiguation**
   - The '>' token kept triggering code generation behaviors
   - Implemented custom token sequence training to fix this