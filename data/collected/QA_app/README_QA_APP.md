# Quality Assurance App

A lightweight web application for quality control of greentext extractions from r/wholesomegreentext subreddit posts. Built with React and Flask.

## Quick Start

### Frontend Setup
```bash
cd frontend
npm install  # Install dependencies
npm run dev  # Start development server
```

### Backend Setup
```bash
cd flask_backend
pip install -r requirements.txt  # Install dependencies
python app.py  # Start Flask server
```

⚠️ **Important**: The backend must be started from the `flask_backend` directory due to relative filepath dependencies.

## Sample Data
The repository includes example data to demonstrate the workflow:
- Two sample images in `data/collected/images/`
- Corresponding transcriptions in `db.json`

## Directory Structure
```
QA_app/
├── README.md           # This file
├── flask_backend/      # Python/Flask backend
│   ├── app.py          # Server entry point
│   └── requirements.txt
├── frontend/           # React frontend
│   ├── package.json
│   └── src/
└── db.json             # Sample transcription data
```

## Technical Notes
- This is a prototype-quality tool built for rapid data curation
- Paths are hardcoded for quick development
- See main project README for context about the data collection pipeline
