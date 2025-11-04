## Building a Release

### Local Development Build (Both Frontend + Backend)

#### Complete Local Setup
```bash
# 1. Start Backend
cd Backend
.\.venv313\Scripts\Activate.ps1  # Windows
# source .venv313/bin/activate    # Mac/Linux

# Install/update dependencies
pip install -r requirements.txt
pip install -r requirements-dev.txt

# Start backend server
uvicorn app.main:app --reload --host 127.0.0.1 --port 8000

# 2. In NEW terminal - Start Frontend
cd Frontend
npm install
npm run dev
```

#### Local Production-like Build
```bash
# Backend - Production mode (no auto-reload)
cd Backend
.\.venv313\Scripts\Activate.ps1
uvicorn app.main:app --host 127.0.0.1 --port 8000

# Frontend - Production build served locally
cd Frontend
npm run build
npm run preview
```

#### Environment Configuration for Local Deploy
**Backend (.env):**
```bash
SUPABASE_URL="https://yourprojectid.supabase.co"
SUPABASE_KEY="your-service-role-key"
OPENAI_API_KEY="your_openai_key"
```

**Frontend (.env):**
```bash
VITE_API_URL=http://127.0.0.1:8000
VITE_SUPABASE_URL=https://yourprojectid.supabase.co
VITE_SUPABASE_ANON_KEY="your-anon-key"
```

#### Verify Local Build Works
```bash
# 1. Check backend is running
curl http://127.0.0.1:8000
# Should return: {"message": "Hello from FastAPI backend!"}

# 2. Visit API docs
# http://127.0.0.1:8000/docs

# 3. Check frontend
# http://localhost:5173 (dev) or http://localhost:4173 (preview)
```

#### Pre-Deploy Testing
```bash
# Backend tests
cd Backend
make format
python -m pytest

# Frontend tests  
cd Frontend
make format
npm test

# Manual testing checklist:
# - [ ] User can sign up/login
# - [ ] Solo crossword works
# - [ ] Battle invites create successfully
# - [ ] All API endpoints respond correctly
```

#### Stop Local Deployment
```bash
# Stop backend: Ctrl+C in backend terminal
deactivate

# Stop frontend: Ctrl+C in frontend terminal
```

### Local Build Troubleshooting

**Backend Issues:**
- Port 8000 already in use: `netstat -ano | findstr :8000` then kill process
- Import errors: Reinstall requirements with `pip install -r requirements.txt`
- Database connection: Verify Supabase credentials in .env

**Frontend Issues:**
- CORS errors: Ensure backend CORS allows localhost:5173
- Build failures: Clear node_modules and reinstall with `npm install`
- Environment variables not loading: Restart dev server after .env changes

**Full Reset (if issues persist):**
```bash
# Backend
cd Backend
deactivate
Remove-Item .venv313 -Recurse -Force  # Windows
py -3.13 -m venv .venv313
.\.venv313\Scripts\Activate.ps1
pip install -r requirements.txt

# Frontend  
cd Frontend
Remove-Item node_modules -Recurse -Force
Remove-Item package-lock.json -Force
npm install
```