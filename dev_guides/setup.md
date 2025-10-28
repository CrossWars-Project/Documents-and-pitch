# Setup
Welcome to Crosswars! This document is your go to source for getting set up and understanding the basic structure for guidelines around contributing to this project. 

Initial setup instructions including tools, repo layout, workflow, commit/pull guidelines, and .env.example

## Repos and DB
Crosswars is split between 3 repos uses Supabase as a db.
### Repos: 
1. Reports and Documentation
2. Backend (you will need to clone this)
3. Frontend (you will need to clone this)
### Supabase:
To gain access to supabase and its keys you will need to email danielleakaella@gmail.com to get access the project id and keys information for your .env file

## Layout 
Reflects current published version of respective repos
/
├─ backend/                 # FastAPI backend
|  ├─.circleci/
|  ├─.pytest_cache/
|  ├─app/
|  |  ├─routes/             # All routes for interacting with database
|  |  |  ├─ crossword.py    # Fetch words and clues. Generate valid puzzles
|  |  |  ├─ invites.py      # Create invite token, support invite join, status transitions
|  |  |  └─ stats.py        # Per-user aggregates, update personal leaderboards
|  |  ├─auth.py             # Get authenticated token
|  |  ├─db.py               # Supabase connection
|  |  └─models.py           # SQLAlchemy ORM define tables
|  ├─tests/                 # Contains all the tests for routes and functions
|  ├─.env
|  ├─venv/
|  ├─.env.example
|  ├─.gitignore
|  ├─Makefile
|  ├─mypy.ini
|  ├─pytest.ini
|  ├─requriements-dev.txt
|  ├─requriements.txt
|  └─README.md
├─ frontend/                # React frontend
|  ├─.circleci/
|  ├─node_modules/
|  ├─public/
|  ├─src/
|  |  ├─components/
|  |  |  ├─ assets/
|  |  |  ├─ BattleScreen/
|  |  |  ├─ Dashboard/
|  |  |  ├─ GuestDashboard/
|  |  |  ├─ HomeScreen/
|  |  |  ├─ LoginSignup/
|  |  |  ├─ SoloPlay/
|  |  |  └─ Stats/
|  |  ├─context/
|  |  ├─api.js
|  |  ├─App.jsx
|  |  ├─index.css
|  |  ├─main.jsx
|  |  ├─router.jsx
|  |  └─supabaseClient.js
|  ├─.env
|  ├─.env/example
|  ├─.eslinterc.cjs
|  ├─.gitignore
|  ├─index.html
|  ├─Makefile
|  ├─package-lock.json
|  ├─package.json
|  ├─README.md
|  └─vite.config.js
└─ reports-and-docs/    # All documentation
   ├─ dev_guides/       # Documentation for Devs
   ├─ user_guides/      # Documentation for Users
   ├─ Reports/          # Weekly reports for Dev team on WIP, Goals, and Progress Updates
   ├─ Cross Wars Working Doc.pdf        # PDF of devs working doc 
   └─ README.md         # General overview and table of content


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
```npm install
npm install react-icons
```
### 3. Set up environement 
- change .env.example to just .env
- in your .env file you should have: 
``` VITE_API_URL=http://127.0.0.1:8000
 VITE_SUPABASE_URL=https://<project-id>.supabase.co
 VITE_SUPABASE_ANON_KEY="anon-key"
```
> email danielleakaella@gmail.com to get dev access to the supabase project to obtian project id and anon key
- if necessary replace http://127.0.0.1:8000 to whatever your backend port is 
### 4. Start Development server
```npm run dev
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
- In you VS terminal run
`python -m venv venv`
- Then activate the virtual environment:
    - Windows: `venv\Scripts\activate`
    - Mac:`venv/bin/activate`

### 3. Install Dependencies
- In your VS terminal run
```pip install -r requirements.txt
pip install -r requirements-dev.txt
```
- If you add new packages later update requirements.txt with
```pip freeze | Out-File -Encoding utf8 requirements.txt
```
### 4. Create a .env File
- The project ID and key can be found in the supabase project settings under API Keys

    SUPABASE_URL="https://yourprojectid.supabase.co"
    SUPABASE_KEY="your-anon-or-service-role-key"
    
### 5. Run the Development Server and Check
- in terminal run 
```uvicorn app.main:app --reload
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
 ```git checkout main
git pull origin main
 ```
2. checkout a branch using checkout or switch
 ```git checkout -b <feature-or-fix>/<description>
```
or
```
    git switch -c <feature-or-fix>/<description>
```
3. Make code changes
    - keep commits small and descriptive
        - commit for feature change
        - commit for new tests you need to write
        - commit for documentation
4. Format, Run tests, Manual test on local host
 ```make format
```
> When applicable run the backend to fully walk through and manually test frontend on local host
5. Push branch to origin
 ```git push origin <branch-name>
 ```
6. Make Pull Request
7. Automated testing/ code review
8. Merge to Main

## Backend Workflow
 1. Start from latest main branch
 ```git checkout main
git pull origin main
 ```
 2. checkout a branch using checkout or switch
 ```git checkout -b <feature-or-fix>/<description>
    git switch -c <feature-or-fix>/<description>
```
 2. Activate venv
 ```venv/Scripts/activate
 ```
    > If you are getting import errors you may need to re-install requirements refer back to the setup instructions above for installs
    > 
    > or if that doest work try
    > `pip install fastapi uvicorn supabase python-dotenv pytest`

    > If your requirements.txt file has become corrupted: 
    > # Delete corrupted file
    > `del requirements.txt`
    >    
    > # Create new clean file with UTF-8 encoding
    > `pip freeze | Out-File -Encoding utf8 requirements.txt`
    >
    > # Verify it's clean
    > `type requirements.txt`
 3. Make code changes
    - keep commits small and descriptive
        - commit for feature change
        - commit for new tests you need to write
        - commit for documentation
 4. Format and Run test
 ```make format
 uvicorn app.main:app --reload
python -m pytest
```
 5. Push branch to origin
 ```git push origin <branch-name>
 ```
 6. End dev session aka leave venv
 ```deactivate
 ```
 7. Open Pull Request 
 8. Automated testing/ code review
 9. Merge to main
