# Setup
Welcome to Crosswars! This document is your go to source for getting set up and understanding the basic structure for guidelines around contributing to this project. 

Initial setup instructions including tools, repo layout, workflow, commit/pull guidelines, and .env.example

## Repos and DB
Crosswars is split between 3 repos uses Supabase as a db.
### Repos: 
**Backend (`/backend`)**
- API routes handle crossword generation, race invites, and user statistics
- Supabase is used for database operations and authentication
- Tests ensure route functionality and data integrity

**Frontend (`/frontend`)** 
- Built with React and Vite for fast development and builds
- Components are organized by feature/screen for maintainability
- Context providers manage global state (authentication, game data)
- `api.js` centralizes all backend API calls

**Documentation (`/reports-and-docs`)**
- Guides for developers and end-users
- Weekly reports track team progress and goals

### Supabase:
To gain access to supabase and its keys you will need to email danielleakaella@gmail.com to get access the project id and keys information for your .env file

## Repository Layout
Reflects current published version of respective repos.
```
/
├─ backend/                 # FastAPI backend
│  ├─ .circleci/            # CI/CD configuration
│  ├─ app/
│  │  ├─ routes/            # API endpoints for database interactions
│  │  │  ├─ crossword.py    # Fetch words/clues, generate puzzles
│  │  │  ├─ invites.py      # Race invites: create tokens, join, status updates
│  │  │  └─ stats.py        # User stats, leaderboards
│  │  ├─ auth.py            # Authentication token management
│  │  ├─ db.py              # Supabase connection setup
│  │  └─ models.py          # SQLAlchemy ORM table definitions
│  ├─ tests/                # Test suite for routes and functions
│  ├─ venv/                 # Python virtual environment
│  ├─ .env                  # Environment variables (not in git)
│  ├─ .env.example          # Template for required environment variables
│  ├─ requirements.txt      # Production dependencies
│  ├─ requirements-dev.txt  # Development dependencies
│  └─ pytest.ini            # Pytest configuration
│
├─ frontend/                # React frontend (Vite)
│  ├─ .circleci/            # CI/CD configuration
│  ├─ public/               # Static assets (favicon, images)
│  ├─ src/
│  │  ├─ components/        # React components organized by feature
│  │  │  ├─ assets/         # Component-specific images/icons
│  │  │  ├─ BattleScreen/   # Race mode gameplay UI
│  │  │  ├─ Dashboard/      # Authenticated user dashboard
│  │  │  ├─ GuestDashboard/ # Guest user view
│  │  │  ├─ HomeScreen/     # Landing page
│  │  │  ├─ LoginSignup/    # Authentication forms
│  │  │  ├─ SoloPlay/       # Solo crossword gameplay UI
│  │  │  └─ Stats/          # User statistics and leaderboards
│  │  ├─ context/           # React Context providers (auth, game state)
│  │  ├─ api.js             # Backend API client functions
│  │  ├─ supabaseClient.js  # Supabase client configuration
│  │  ├─ router.jsx         # React Router route definitions
│  │  └─ App.jsx            # Root application component
│  ├─ .env                  # Environment variables (not in git)
│  ├─ .env.example          # Template for required environment variables
│  ├─ package.json          # Node dependencies and scripts
│  └─ vite.config.js        # Vite build configuration
│
└─ reports-and-docs/        # Project documentation
   ├─ dev_guides/           # Developer guides (setup, testing, deployment)
   ├─ user_guides/          # End-user documentation
   ├─ Reports/              # Weekly progress reports for dev team
   ├─ Cross Wars Working Doc.pdf  # Development working document
   └─ README.md             # Documentation overview and navigation
```


--- 
## Frontend Setup

### Prerequisits
Make sure you have the following installed or access to **before starting**:
- [Node.JS] (https://nodejs.org/en/download/current)
    - `node -v`
    - `npm -v`
- Backend running locally (to check connection)
- A [Supabase](https://supabase.com) project (ask the team for the credentials)
### 1. Clone Repo
### 2. Install dependencies
- install react, vite, icons, and all other packages with
```bash
npm install
npm install react-icons
```
### 3. Set up environement 
- change .env.example to just .env
- in your .env file you should have: 
```bash
VITE_API_URL=http://127.0.0.1:8000
VITE_SUPABASE_URL=https://<project-id>.supabase.co
VITE_SUPABASE_ANON_KEY="anon-key"
```
> email danielleakaella@gmail.com to get dev access to the supabase project to obtian project id and anon key
- if necessary replace http://127.0.0.1:8000 to whatever your backend port is 
### 4. Start Development server
```bash
npm run dev
```


---
## Backend Setup
### Prerequisites

Make sure you have the following installed or access to **before starting**:

- [Python 3.10+](https://www.python.org/downloads/)
- [Git](https://git-scm.com/downloads)
- [Chocolaty] (https://chocolatey.org/install) (for Windows users to use `make format`)
- [VS Code](https://code.visualstudio.com/)
- A [Supabase](https://supabase.com) project (ask the team for the credentials)

---

### 1. Clone the Repository

### 2. Create and Activate a Virtual Environment
- In you VS terminal run: `py -3.13 -m venv .venv313`
- Then activate the virtual environment:
    - Windows: `.\.venv313\Scripts\Activate.ps1`
    - Mac:`source .venv313/bin/activate`

### 3. Install Dependencies
- In your VS terminal run
```bash
pip install -r requirements.txt
pip install -r requirements-dev.txt
```
- If you add new packages later update requirements.txt with
```bash
pip freeze | Out-File -Encoding utf8 requirements.txt
```
### 4. Create a .env File
- The project ID and key can be found in the supabase project settings under API Keys

    SUPABASE_URL="https://yourprojectid.supabase.co"
    SUPABASE_KEY="your-anon-or-service-role-key"
    
### 5. Run the Development Server and Check
- in terminal run 
```bash
uvicorn app.main:app --reload
```
 - visit http://127.0.0.1:8000 to check if its working, should see { "message": "Hello from FastAPI backend!" }
 

### Additional setup notes
- If vev prompts use venv interpreter `.venv/Scripts/python.exe`
- If no prompt but having issue:
    1. `Ctrl + Shift + P`
    2. Type Python:Select Interpreter
    3. choose the one that says venv and includes python.exe
 --

## Frontend Workflow
1. Start from latest main branch
 ```bash
git checkout main
git pull origin main
 ```
2. checkout a branch using checkout or switch
 ```bash
git checkout -b <feature-or-fix>/<description>
```
or
```bash
git switch -c <feature-or-fix>/<description>
```
3. Make code changes
    - keep commits small and descriptive
        - commit for feature change
        - commit for new tests you need to write
        - commit for documentation
4. Format, Run tests, Manual test on local host
 ```bash
make format
```
> When applicable run the backend to fully walk through and manually test frontend on local host
5. Push branch to origin
 ```bash
 git push origin <branch-name>
 ```
6. Make Pull Request
7. Automated testing/ code review
8. Merge to Main

## Backend Workflow
 1. Start from latest main branch
 ```bash
 git checkout main
git pull origin main
 ```
 2. checkout a branch using checkout or switch
 ```bash
 git checkout -b <feature-or-fix>/<description>
 git switch -c <feature-or-fix>/<description>
```
 2. Activate venv
 ```bash
 venv/Scripts/activate
 ```
> If you are getting import errors you may need to re-install requirements refer back to the setup instructions above for installs
> 
> or if that doesn't work try: `pip install fastapi uvicorn supabase python-dotenv pytest`

> If your requirements.txt file has become corrupted: 
> **Delete corrupted file**:`del requirements.txt`
> **Create new clean file with UTF-8 encoding**: `pip freeze | Out-File -Encoding utf8 requirements.txt`
> **Verify it's clean**: `type requirements.txt`
 3. Make code changes
    - keep commits small and descriptive
        - commit for feature change
        - commit for new tests you need to write
        - commit for documentation
 4. Format and Run test
 ```bash
 make format
 uvicorn app.main:app --reload
 python -m pytest
```
 5. Push branch to origin
 ```bash
 git push origin <branch-name>
 ```
 6. End dev session aka leave venv
 ```bash
 deactivate
 ```
 7. Open Pull Request 
 8. Automated testing/ code review
 9. Merge to main
