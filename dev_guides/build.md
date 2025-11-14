# Building and Running CrossWars

This guide covers building and running CrossWars locally for development and production-like testing. For initial setup, see [setup.md](setup.md). For testing details, see [testing.md](testing.md).

## Quick Start (Daily Development)

**Most common workflow** - run these in separate terminals:

### Terminal 1: Backend Development Server
```bash
cd Backend
.\.venv313\Scripts\Activate.ps1  # Windows
# source .venv313/bin/activate    # Mac/Linux

uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```
*Auto-reloads on code changes for faster development*

### Terminal 2: Frontend Development Server
```bash
cd Frontend
npm run dev
```
*Hot-reload shows changes instantly in browser*

### Verify Build is Working
- Backend API: http://127.0.0.1:8000 → Should show "Hello from FastAPI backend!"
- Frontend: http://localhost:5173 → Should show CrossWars login page
- API Docs: http://127.0.0.1:8000/docs → Interactive API testing interface

---

## Production-Like Local Build

**When to use:** Testing performance, final validation before deployment, debugging production-specific issues.

### Backend (No Auto-Reload)
```bash
cd Backend
.\.venv313\Scripts\Activate.ps1
uvicorn app.main:app --host 127.0.0.1 --port 8000
```

### Frontend (Optimized Build)
```bash
cd Frontend
npm run build          # Creates optimized build in dist/
npm run preview        # Serves built app on port 4173
```

**Access:** Frontend at http://localhost:4173 (production-optimized)

---

## Environment Configuration

### When Environment Changes Are Needed
- Switching between Supabase projects (dev/prod)
- Testing with different API endpoints
- Using different external service accounts

### Backend Environment (.env)
```bash
SUPABASE_URL="https://yourprojectid.supabase.co"
SUPABASE_KEY="your-service-role-key"
OPENAI_API_KEY="your_openai_key"
```

### Frontend Environment (.env)
```bash
VITE_API_URL=http://127.0.0.1:8000
VITE_SUPABASE_URL=https://yourprojectid.supabase.co
VITE_SUPABASE_ANON_KEY="your-anon-key"
```

**Important:** Restart both servers after changing environment variables.

---

## Build Workflows

### 1. Feature Development
```bash
# Start development servers (auto-reload enabled)
cd Backend && .\.venv313\Scripts\Activate.ps1 && uvicorn app.main:app --reload
cd Frontend && npm run dev

# Code, test in browser, iterate quickly
```

### 2. Pre-Commit Validation
```bash
# Format code (see testing.md for test commands)
cd Backend && make format
cd Frontend && make format

# Test production build works
cd Frontend && npm run build && npm run preview
```

### 3. Dependency Updates
```bash
# After pulling code that changes dependencies
cd Backend && pip install -r requirements.txt
cd Frontend && npm install

# Restart servers to use new dependencies
```

---

## Stopping Services

### Graceful Shutdown
```bash
# In each terminal window:
Ctrl+C                 # Stop the server
deactivate            # Exit Python virtual environment (backend only)
```

### Force Stop (Unresponsive Servers)
- Close terminal windows
- Kill processes via Task Manager (Windows) or Activity Monitor (Mac)

---

## Build Troubleshooting

### Backend Build Issues

**Port 8000 already in use:**
```bash
# Windows
netstat -ano | findstr :8000
taskkill /PID <PID> /F

# Mac/Linux
lsof -ti:8000 | xargs kill -9
```

**Import/dependency errors after pulling new code:**
```bash
cd Backend
pip install -r requirements.txt  # Install missing dependencies
pip install -r requirements-dev.txt  # Install dev tools
```

**Database connection failures:**
- Verify `.env` file has correct Supabase credentials
- Check Supabase project is active and accessible
- Ensure using service role key (not anon key) for backend

### Frontend Build Issues

**CORS errors in browser:**
- Ensure backend is running on expected port (8000)
- Verify backend CORS settings allow localhost:5173
- Check `VITE_API_URL` in frontend `.env` matches backend URL

**npm build failures:**
```bash
cd Frontend
# Clear cache and reinstall
rm -rf node_modules package-lock.json  # Mac/Linux
Remove-Item node_modules, package-lock.json -Recurse -Force  # Windows
npm install
```

**Environment variables not working:**
- Restart dev server after changing `.env` files
- Ensure frontend variables start with `VITE_`
- Check browser dev tools for actual values being used

---

## Build vs Test vs Setup

- **This guide (build.md):** Running and building the application
- **[testing.md](testing.md):** Running tests, test structure, adding new tests
- **[setup.md](setup.md):** Initial project setup, repo access, tool installation

For testing commands and procedures, see [testing.md](testing.md).