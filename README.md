# WholesomeGreentext: Fine-tuning Language Models for Style Transfer

I built this project as a hands-on educational project about idiosyncratic style transfer of LLMs (or LMs) and presented the results at AI Tinkerers Munich to about a hundred people.

I have built on top of the work of [TinyStories](https://arxiv.org/abs/2305.07759) who pre-trained LMs with up to 33M parameters which still produced coherent stories. 

They were able to accomplish this by training on a limited vocabulary corpus (synthetic childrens stories).

I wanted to evaluate if these capabilities persist style transfer.

To this end I focused on reproducing the unique linguistic style of 4chan greentext stories.

To avoid toxicity, I started with scraping data from the '/r/wholesomegreentext' and filtering out inappropriate content.

WholesomeGreentext was an educational project focused on  The project explored several key areas of ML engineering:

- Data collection and preprocessing from Reddit using OCR
- Synthetic data generation and curation
- Fine-tuning of lightweight transformer models
- Custom tokenization strategies
- Model deployment and web application development
- Presentation and communication of results to a real audience


## Technical Implementation

### Data Pipeline

The project utilized two main data sources:

1. **Real Data Collection**
   - Scraped screenshots from r/wholesomegreentexts using Reddit's Pushshift API (now defunct)
   - Implemented OCR using Pytesseract for text extraction
   - Built a Flask/React QA application for manual data curation

2. **Synthetic Data Generation**
   - Leveraged OpenAI's API for synthetic data generation
   - Created specialized datasets:
     - Standard 4chan-style greentexts
     - Simplified vocabulary greentexts (TinyStories-compatible)
     - Genre-specific "thank you" greentexts
   - Total dataset size: 239k examples
   - All datasets available on [Huggingface](https://huggingface.co/maxmyn)

### Model Development

The project explored two main model architectures:

1. **TinyStories-based Models**
   - Fine-tuned the largest TinyStories model
   - Focused on simplified vocabulary and specific genres
   - Best performance achieved with "thank you" genre-specific training

2. **DistilGPT-2-based Models**
   - Implemented custom token sequence training (`<|4chanGtxStart|>`)
   - Successfully mitigated code generation bias from '>' token

### Key Findings

- Data quality and specificity proved more important than quantity
- Genre-specific training outperformed general training despite smaller dataset size (5x difference)
- Custom token sequences significantly improved generation quality
- Vocabulary constraints from base models heavily influenced fine-tuning performance

### Infrastructure

- Frontend: React, deployed via Railway
- Model Endpoints: Huggingface
- Development Environment: Kaggle and Google Colab for training
- Data Storage: Huggingface Datasets

## Technical Challenges and Solutions

1. **Dataset Size Limitations**
   - Challenge: Insufficient real-world data
   - Solution: Implemented synthetic data generation pipeline with OpenAI API

2. **Vocabulary Control**
   - Challenge: Managing text complexity while maintaining style
   - Solution: Created filtered datasets matching TinyStories vocabulary

3. **Token Disambiguation**
   - Challenge: '>' token triggering code generation
   - Solution: Implemented custom token sequence training

## Future Improvements

- Experiment with larger base models while maintaining reasonable inference costs
- Implement more sophisticated content filtering
- Explore multi-task learning for better style transfer
- Investigate few-shot learning approaches

## Skills Demonstrated

- ML Engineering
- Data Collection and Preprocessing
- Model Fine-tuning
- API Development
- Web Development
- MLOps
- Data Analysis

## Repository Structure

```
├── QA_App/
│   ├── flask_backend/
│   └── frontend/
├── ml_stuff/
│   ├── data/
│   │   └── synthetic/
│   └── training/
└── README.md
```

## Setup Instructions
These you find in the QA app folder 
1. Backend Setup:
```bash
cd QA_App/flask_backend
pip install -r requirements.txt
python app.py
```

2. Frontend Setup:
```bash
cd QA_App/frontend
npm install
npm run dev
```