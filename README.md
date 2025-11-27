# LLM Analysis Quiz Solver

A complete backend system for solving LLM analysis quizzes automatically. The system uses FastAPI for the server, Playwright for JS-rendered pages, and intelligent heuristics to extract, analyze, and submit quiz answers.

## Features

- FastAPI Backend — Async REST API with /solve endpoint
- Playwright Integration — Handles JavaScript-rendered quiz pages
- Multi-format Support — CSV, PDF, Excel, JSON, images, text files
- Automatic Chain Solving — Solves multi-step quizzes automatically
- Multiple Answer Strategies — File parsing, regex, pattern matching
- Secure Secret Validation — Prevents unauthorized access
- Logging & Error Handling — Robust retries and debugging support

## Project Structure


Project-LLM-Analysis-Quiz/
├── LICENSE
├── README.md
├── .gitignore
├── requirements.txt
├── Dockerfile
├── run.py
│
├── app/
│   ├── _init_.py
│   ├── config.py      # Environment variables, settings
│   ├── main.py        # FastAPI entry point
│   ├── scraper.py     # Playwright automation & rendering
│   ├── solver.py      # Answer computation logic
│   ├── utils.py       # File parsing helpers
│
└── tests/
    ├── test_main.py
    ├── test_utils.py


## Installation

### Prerequisites

- Python 3.10+
- pip

### Setup

bash
cd llm-quiz-agent
python -m venv venv
venv\Scripts\activate      # Windows
# or: source venv/bin/activate (Mac/Linux)

pip install -r requirements.txt
playwright install


Create environment file:

bash
cp .env.example .env
# Edit .env and set SECRET and EMAIL


## Usage

### Run the Server

bash
python -m app.main


or

bash
uvicorn app.main:app --host 0.0.0.0 --port 8000 --reload


Server runs at:


http://0.0.0.0:8000


## API Endpoints

### POST /solve

Request
json
{
  "email": "your-email@example.com",
  "secret": "your-secret-key",
  "url": "https://quiz-url.com/quiz-123"
}


Response
json
{
  "success": true,
  "message": "All quizzes completed successfully",
  "result": {
    "quizzes_solved": [
      {
        "quiz_number": 1,
        "url": "https://quiz-url.com/quiz-123",
        "answer": 42.5,
        "submission_result": {
          "correct": true,
          "url": "https://quiz-url.com/quiz-456"
        }
      }
    ],
    "total_quizzes": 1,
    "success": true
  }
}


### GET /health

json
{ "status": "healthy" }


### GET /

Returns API information.

## How It Works

1. Validates request secret and quiz URL  
2. Loads the quiz page using Playwright (handles JS rendering)  
3. Extracts question, data links, and submission endpoints  
4. Downloads and analyzes data files  
5. Computes answers using multiple logic strategies  
6. Submits answer and follows next quiz link  
7. Stops when chain ends or timeout is reached  

## Configuration

Modify .env:


SECRET=<your-secret>
EMAIL=<your-email>
PORT=8000
QUIZ_TIMEOUT=180
MAX_RETRIES=3
LOG_LEVEL=INFO


## Testing

bash
pytest tests/
pytest --cov=app tests/


## Troubleshooting

Playwright missing browsers
bash
playwright install chromium


Port already in use
bash
netstat -ano | findstr :8000
taskkill /PID <PID> /F


Import issues
bash
python -m app.main


## License

MIT License — see LICENSE file for full details.

## Contributing

1. Fork the repository  
2. Create a feature branch  
3. Make changes and add tests  
4. Open a pull request  
