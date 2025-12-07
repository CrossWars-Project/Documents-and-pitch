# Setup
Welcome to Crosswars! This document is your go-to source for getting set up and understanding the basic structure of guidelines for contributing to this project.

Initial setup instructions including tools, repo layout, workflow, commit/pull guidelines, and .env.example

## Repos and DB
Crosswars is split between 3 repos and uses Supabase as a db.
### Repos:
**[Backend](https://github.com/CrossWars-Project/Backend)**
- API routes handle crossword generation, race invites, and user statistics
- Supabase is used for database operations and authentication
- Tests ensure route functionality and data integrity

**[Frontend](https://github.com/CrossWars-Project/FrontEnd)**
- Built with React and Vite for fast development and builds
- Components are organized by feature/screen for maintainability
- Context providers manage global state (authentication, game data)
- `api.js` centralizes all backend API calls

**Documentation (`/reports-and-docs`)**
- Guides for developers and end-users
- Weekly reports track team progress and goals

### Supabase and OPENAI:
To gain access to Supabase and its keys, you will need to email danielleakaella@gmail.com to get access to the project ID and keys information for your .env file.

## Repository Layout
Reflects the current published version of respective repos.
```
/
├─ backend/                 # FastAPI backend
│  ├─ .github/              # GitHub workflows and automated jobs
│  ├─ app/
│  │  ├─ routes/            # API endpoints for database 
interactions
│  │  ├─ generator.py
│  │  └─ main.py
│  ├─ tests/                # Test suite for routes and functions
│  ├─ .env                  # Environment variables (not in git)
│  ├─ .env.example          # Template for required environment variables
│  ├─ README.md
│  ├─ requirements.txt      # Production dependencies
│  └─ requirements-dev.txt  # Development dependencies
│
├─ frontend/                # React frontend (Vite)
│  ├─ public/               # Static assets (favicon, images)
│  ├─ src/
│  │  ├─ components/        # React components organized by feature
│  │  ├─ context/           # React Context providers (auth, game state)
│  │  ├─ api.js             # Backend API client functions
│  │  └─ config.js          # API configuration
│  ├─ .env                  # Environment variables (not in git)
│  ├─ .env.example          # Template for required environment variables
│  └─  package.json          # Node dependencies and scripts
│
└─ reports-and-docs/        # Project documentation
  ├─ dev_guides/           # Developer guides
  ├─ user_guides/          # End-user documentation
  ├─ Reports/              # Weekly progress reports for the dev team
  ├─ Cross Wars Working Doc.pdf  # Development working document
  └─ README.md             # Documentation overview and navigation
```


---
## Frontend Setup

### Prerequisites
Make sure you have the following installed or access to **before starting**:
- [Node.JS 22.12+] (https://nodejs.org/en/download/current)
   - `node -v`
   - `npm -v`
- Backend running locally (to check connection)
- A [Supabase](https://supabase.com) project (ask the team for the credentials)

### 1. Clone Repo
[Frontend Repo](https://github.com/CrossWars-Project/Frontend#)
- Alternatively, you can paste this command in your terminal: git clone https://github.com/CrossWars-Project/Frontend.git

### 2. Install dependencies
` cd frontend `
- install all packages with
```bash
npm install
npm install react-icons
```
**Important:** You must run `npm install react-icons` after `npm install` to ensure all icon dependencies are properly installed.

### 3. Set up the environment
- change .env.example to just .env
- in your .env file, you should have:
```bash
VITE_API_URL=http://127.0.0.1:8000 
VITE_SUPABASE_URL=https://<project-id>.supabase.co
VITE_SUPABASE_ANON_KEY="anon-key"
```
> email dmaddo@uw.edu or akpettis@uw.edu to get dev access to the supabase project to obtain project ID and anon key
- If necessary, replace http://127.0.0.1:8000 to whatever your backend port is
### 4. Start the Development server
```bash
npm run dev
```


---
## Backend Setup
### Prerequisites

Make sure you have the following installed or access to **before starting**:

- [Python 3.13+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)
- [Chocolaty] (https://chocolatey.org/install) (for Windows users to use `make format`)
- [VS Code](https://code.visualstudio.com/)
- A [Supabase](https://supabase.com) project (ask the team for the credentials or use your own project)

---

### 1. Clone the Repository
[Backend Repo](https://github.com/CrossWars-Project/Backend#)
- Alternatively, you can paste this command in your terminal: git clone https://github.com/CrossWars-Project/Backend.git

### 2. Create and Activate a Virtual Environment
` cd backend `
- In VS terminal (different from frontend terminal), run:
   - Windows: `py -3.13 -m venv .venv313`
   - Mac: `python3 -m venv .venv313`
- Then activate the virtual environment:
   - Windows: `.venv313\Scripts\Activate.ps1`
   - Mac:`source .venv313/bin/activate`
- Once you are in venv, make sure to test you python version by running python --version or py --version or python3 --version depending on your system.
- NOTE: If Python version is not 3.12+, you will need to update the python on your system and restart the process so the chnages take place. 

### 3. Install Dependencies
- In your VS terminal, run
```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt
```
- If you add new packages later, update requirements.txt with
```bash
pip freeze | Out-File -Encoding utf8 requirements.txt
```
### 4. Change .env.example to .env and add the Keys
- change .env.example to just .env
- in your .env file, you should have:
```bash
OPENAI_API_KEY="your_key_here"
SUPABASE_URL="https://<project-id>.supabase.co"
SUPABASE_KEY="service-roll-key"
```
**Important:** 
- Do NOT use quotes around the API key!
- The `.env` file should be in the `backend/` directory (not `backend/app/`)

> email dmaddo@uw.edu or akpettis@uw.edu to get dev access to the supabase project to obtain project ID and anon key
   
   
### 5. Run the Development Server and Check
- in the terminal run
```bash
uvicorn app.main:app --reload
```
- visit http://127.0.0.1:8000 to check if it's working, should see { "message": "Hello from FastAPI backend!" }


### Backend setup troubleshooting
- If vev prompts use venv interpreter `.venv313/Scripts/python.exe`
- If no prompt, but having an issue:
   1. `Ctrl + Shift + P`
   2. Type Python:Select Interpreter
   3. Choose the one that says venv and includes python.exe
- If you are missing a key or need the .env file, email danielleakaella@gmail.com
