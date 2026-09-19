# Resume Parser

An LLM-powered Python tool that parses resumes, extracts job requirements, and ranks candidates against a job description.

It reads **PDF** and **DOCX** files, uses **Groq** with the `openai/gpt-oss-20b` model to extract and compare information, and validates structured responses with **Pydantic**.

## Features

- Process multiple resumes from a local folder.
- Extract contact details, skills, work experience, education, projects, and certifications.
- Extract required and preferred skills, minimum experience, education requirements, and responsibilities from a job description.
- Generate match explanations covering matching skills, missing skills, experience requirements, and a short verdict.
- Request an overall match score from 0 to 100 for each candidate.
- Sort candidates by score and display the top two and lowest two results in the terminal.

## How It Works

```text
Job description  --> Structured job requirements
PDF/DOCX resumes --> Text extraction --> Structured candidate profiles
Job requirements + Candidate profiles --> LLM comparison --> Ranked results
```

The script makes one model request to parse the job description and two requests per resume: one for parsing and one for matching. It includes five-second pauses after each resume-parsing and matching request.

## Tech Stack

| Tool | Purpose |
| --- | --- |
| Python | Application logic |
| Groq API | LLM inference |
| Pydantic | Structured data models and response validation |
| pypdf | PDF text extraction |
| python-docx | DOCX paragraph and table extraction |
| python-dotenv | Loading the API key from `.env` |
| uv | Dependency and environment management |

## Getting Started

### Prerequisites

- Git
- [uv](https://docs.astral.sh/uv/getting-started/installation/)
- Python 3.11 or newer; the repository's `.python-version` selects Python 3.14.
- A [Groq API key](https://console.groq.com/keys) and an internet connection.

### 1. Clone the repository and install dependencies

```bash
git clone https://github.com/Raed-Tanwar/Resume-Parser.git
cd Resume-Parser
uv sync
uv add pypdf python-docx
```

- `pypdf` extracts text from PDF resumes.
- `python-docx` extracts text from Word (`.docx`) resumes.

### 2. Configure your API key

Create a file named `.env` in the project root:

```env
GROQ_API_KEY=your_groq_api_key_here
```

The `.env` file is ignored by Git. Do not put your API key directly in the source code or commit it to the repository.

### 3. Set the job description

Open `resume_parser.py` and replace the existing text assigned to `job_description` with the role you want to evaluate:

```python
job_description = """
Paste your job description here.
Include the role, required skills, experience, and qualifications.
"""
```

### 4. Add resumes

Place the resumes you want to evaluate directly inside the `resumes/` folder:

```text
resumes/
├── candidate_1.pdf
└── candidate_2.docx
```

Only `.pdf` and `.docx` files are processed. Other file types and nested folders are not processed.

### 5. Run the script

Run this command from the project root:

```bash
uv run python resume_parser.py
```

The terminal shows each resume being processed and its match score. After processing, it prints **TOP 2 CANDIDATES** and **LOWEST 2 CANDIDATES**, with names, scores, and matching details.

The main workflow lives in `resume_parser.py`; the package under `src/` is currently a starter scaffold.

## Project Structure

```text
Resume-Parser/
├── resume_parser.py       # Main parsing, matching, and ranking workflow
├── resumes/               # Input PDF and DOCX resumes
├── src/
│   └── resume_parser/     # Package scaffold
│       └── __init__.py
├── .env                   # Local API key; create this file yourself
├── .gitignore
├── .python-version
├── pyproject.toml         # Project metadata and dependencies
├── uv.lock                # Dependency lockfile
└── README.md
```

## Future Improvements

- Add a web interface for uploading resumes and job descriptions.
- Export candidate rankings and match details to CSV or JSON.
- Add OCR support for scanned PDF resumes.
