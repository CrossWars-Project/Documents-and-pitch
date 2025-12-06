# Contributing to CrossWars

This guide covers the complete development workflow for contributing to CrossWars: building, testing, and releasing the software. For initial setup, see [setup.md](setup.md).

---

## Table of Contents
1. [Building the Software](#building-the-software)
2. [Testing the Software](#testing-the-software)
3. [Adding New Tests](#adding-new-tests)
4. [Development Workflow](#development-workflow)
5. [Building a Release](#building-a-release)
6. [Troubleshooting](#troubleshooting)

---

## Building the Software

CrossWars consists of two main components: a FastAPI backend and a React frontend. Both must be running for full functionality.

### Quick Start (Daily Development)

Run these commands in separate terminal windows:

#### Terminal 1: Backend Development Server
```bash
cd Backend
.\.venv313\Scripts\Activate.ps1  # Windows
# source .venv313/bin/activate    # Mac/Linux

uvicorn app.main:app --reload --host 127.0.0.1 --port 8000
```
*The `--reload` flag enables auto-reload on code changes for faster development.*

#### Terminal 2: Frontend Development Server
```bash
cd Frontend
npm run dev
```
*Vite provides hot-reload, showing changes instantly in your browser.*

**Important:** Ensure `VITE_API_URL` in `Frontend/.env` is either empty or set to `http://127.0.0.1:8000` for local development.

#### Generate Daily Crosswords (Optional)

Only needed when testing Solo Play or Battle Mode features:

```bash
cd Backend
# Windows
.\.venv313\Scripts\Activate.ps1
Invoke-WebRequest -Uri "http://127.0.0.1:8000/crossword/generate-daily" -Method POST

# Mac/Linux
source .venv313/bin/activate
curl -X POST http://127.0.0.1:8000/crossword/generate-daily
```

Takes 30-60 seconds to generate both solo and battle crosswords.

**For advanced crossword testing workflows** (custom themes, clearing crosswords, testing error states), see [Testing_crosswords.md](Testing_crosswords.md).

### Verify Build is Working

After starting both servers, verify everything is running:

- **Backend API:** http://127.0.0.1:8000 → Should display `{"message": "Hello from FastAPI backend!"}`
- **Frontend:** http://localhost:5173 → Should show CrossWars login page
- **API Docs:** http://127.0.0.1:8000/docs → Interactive API testing interface

### Production-Like Local Build

Use these commands when testing performance, validating before deployment, or debugging production-specific issues.

#### Backend (No Auto-Reload)
```bash
cd Backend
.\.venv313\Scripts\Activate.ps1  # Windows
# source .venv313/bin/activate    # Mac/Linux
uvicorn app.main:app --host 127.0.0.1 --port 8000
```

#### Frontend (Optimized Build)
```bash
cd Frontend
npm run build          # Creates optimized build in dist/
npm run preview        # Serves built app on port 4173
```

**Access the production build at:** http://localhost:4173

### Environment Configuration

#### Switching Between Local and Deployed Backends

The frontend can connect to either your local backend OR the deployed production backend. Configure this via `VITE_API_URL` in `Frontend/.env`.

**Use Local Backend (Default for Development):**
```bash
# Frontend/.env
VITE_API_URL=http://127.0.0.1:8000
```
- **Use when:** Developing features, testing local changes, running full stack locally
- **Requires:** Local backend running
- **Benefits:** Instant feedback, can debug backend, no internet dependency

**Use Deployed Backend (Production Testing):**
```bash
# Frontend/.env  
VITE_API_URL=https://backend-ezw4.onrender.com
```
- **Use when:** Testing against production data, testing frontend without running backend locally
- **Requires:** Deployed backend must be accessible
- **Benefits:** Test with real production environment, validate CORS configuration

**After changing `VITE_API_URL`:** Restart the development server (`Ctrl+C`, then `npm run dev`) or rebuild for preview (`npm run build && npm run preview`).

#### Environment Files Reference

**Backend Environment (`Backend/.env`):**
```bash
SUPABASE_URL="https://yourprojectid.supabase.co"
SUPABASE_KEY="your-service-role-key"
OPENAI_API_KEY="your-openai-key"
```

**Frontend Environment (`Frontend/.env`):**
```bash
VITE_API_URL=http://127.0.0.1:8000
VITE_SUPABASE_URL=https://yourprojectid.supabase.co
VITE_SUPABASE_ANON_KEY="your-anon-key"
```

**Important:** Always restart both servers after modifying environment variables.

### Stopping Services

#### Graceful Shutdown
```bash
# In each terminal window:
Ctrl+C                 # Stop the server
deactivate            # Exit Python virtual environment (backend only)
```

#### Force Stop (Unresponsive Servers)
- Close terminal windows
- Kill processes via Task Manager (Windows) or Activity Monitor (Mac)

---

## Testing the Software

CrossWars uses pytest for backend testing and Jest for frontend testing. All tests should pass before submitting a pull request.

### Running Backend Tests

```bash
cd Backend
.\.venv313\Scripts\Activate.ps1  # Windows
# source .venv313/bin/activate    # Mac/Linux

python -m pytest                  # Run all tests
python -m pytest -v               # Verbose output
python -m pytest tests/test_auth.py  # Run specific test file
```

**Test Requirements:**
- Backend server should NOT be running when executing tests
- Tests use a mock Supabase client (configured automatically via `TESTING=1` environment variable)
- All database operations are mocked to avoid external dependencies

### Running Frontend Tests

```bash
cd Frontend
npm test                    # Run all tests
npm test -- --watch        # Run tests in watch mode
npm test -- BattleInvite   # Run tests matching pattern
```

**Test Requirements:**
- Frontend tests use Jest with React Testing Library
- Tests mock API calls and external dependencies
- Backend server is NOT required for frontend unit tests

### Code Formatting

Format code before committing to maintain consistency:

**Backend:**
```bash
cd Backend
make format     # Runs Black (formatter) and isort (import sorter)
```

**Frontend:**
```bash
cd Frontend
make format     # Runs ESLint with auto-fix
```

### Manual Testing

For features with UI components or complex interactions:

1. Start both backend and frontend servers (see [Building the Software](#building-the-software))
2. Navigate to http://localhost:5173 in your browser
3. Test the feature end-to-end as a user would
4. Check browser console (F12) for errors
5. Verify API calls in Network tab if needed

### Testing Crossword Features

For detailed crossword testing workflows (generating specific modes, custom themes, testing error states), see **[Testing_crosswords.md](Testing_crosswords.md)**.

---

## Adding New Tests

### Backend Test Conventions

**File naming:** `test_<module_name>.py`
- Example: `tests/test_battles.py` for testing `app/routes/battles.py`

**Test function naming:** `test_<feature>_<scenario>`
- Example: `test_create_invite_success()`, `test_accept_invite_already_accepted()`

**Test structure:**
```python
def test_feature_scenario(client):
    """Clear description of what is being tested."""
    # Arrange - Set up test data
    test_data = {"key": "value"}
    
    # Act - Perform the action
    response = client.post("/endpoint", json=test_data)
    
    # Assert - Verify the results
    assert response.status_code == 200
    assert response.json()["success"] is True
```

**Test harness:** Uses pytest with fixtures defined in `tests/conftest.py`
- `client` fixture: Provides TestClient for making API requests
- `mock_supabase` fixture: Automatically activated for all tests

**Location:** All backend tests go in `Backend/tests/`

### Frontend Test Conventions

**File naming:** `<ComponentName>.test.jsx`
- Example: `BattleInvite.test.jsx` for testing `BattleInvite.jsx`
- Place test files in the same directory as the component

**Test structure:**
```javascript
describe('ComponentName', () => {
  test('should render correctly', () => {
    // Arrange
    render(<ComponentName />);
    
    // Act & Assert
    expect(screen.getByText('Expected Text')).toBeInTheDocument();
  });
  
  test('should handle user interaction', () => {
    // Arrange
    render(<ComponentName />);
    
    // Act
    fireEvent.click(screen.getByRole('button'));
    
    // Assert
    expect(mockFunction).toHaveBeenCalled();
  });
});
```

**Test harness:** Uses Jest with React Testing Library
- Mock API calls using Jest mocks
- Mock config and images (see `Frontend/__mocks__/`)

**Location:** Place test files alongside components in `Frontend/src/components/`

### Running New Tests

After adding tests, verify they work:

**Backend:**
```bash
pytest tests/test_your_new_file.py -v
```

**Frontend:**
```bash
npm test -- YourComponentName
```

---

## Development Workflow

Follow this workflow when contributing code to CrossWars.

### 1. Start from Latest Main

```bash
git switch main
git pull origin main
```

### 2. Create a Feature Branch

Use descriptive branch names following this pattern:

```bash
git switch -c feature/<description>    # For new features
git switch -c fix/<description>        # For bug fixes
git switch -c docs/<description>       # For documentation updates
```

Example: `git checkout -b feature/battle-mode-stats`

### 3. Set Up Development Environment

**Backend:**
```bash
cd Backend
.\.venv313\Scripts\Activate.ps1  # Windows
# source .venv313/bin/activate    # Mac/Linux

# If dependencies changed, reinstall:
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

**Frontend:**
```bash
cd Frontend
# If dependencies changed, reinstall:
npm install
```

### 4. Make Code Changes

Keep commits small and focused:
- One commit for the feature/fix implementation
- Separate commit for new tests
- Separate commit for documentation updates

Example commit messages:
```bash
git commit -m "Add battle completion endpoint"
git commit -m "Add tests for battle completion"
git commit -m "Update API docs for battle endpoint"
```

### 5. Format and Test

Before pushing, always format code and run tests:

**Backend:**
```bash
make format
python -m pytest
```

**Frontend:**
```bash
make format
npm test
```

**Manual testing:** Start both servers and verify your changes work end-to-end.

### 6. Push Branch

```bash
git push origin <branch-name>
```

### 7. Create Pull Request

1. Go to GitHub repository
2. Click "Pull requests" → "New pull request"
3. Select your branch
4. Be clear and concise, use screenshots for UI changes
5. Request reviews from team members

### 8. Code Review and Merge

- Address any review comments
- Ensure all automated tests pass (GitHub Actions)
- Once approved, merge to main using "Squash and merge"

### 9. Clean Up

After merge:
```bash
git switch main
git pull origin main
git branch -d <your-branch-name>  # Delete local branch
```

**Backend only:** Remember to deactivate virtual environment:
```bash
deactivate
```

---

## Building a Release

Follow these steps when preparing a production release.

### Pre-Release Checklist

Before building a release, complete these tasks:

1. **Ensure all tests pass:**
   ```bash
   cd Backend && python -m pytest
   cd Frontend && npm test
   ```

3. **Build and test locally:**
   ```bash
   cd Frontend
   npm run build
   npm run preview  # Test production build locally
   ```

4. **Verify environment variables:**
   - Ensure production `.env` files are configured correctly
   - Double-check API URLs, keys, and credentials

### Creating the Release

1. **Deploy backend:**
   - Push to main branch (triggers auto-deploy on Render)
   - Verify deployment: https://backend-ezw4.onrender.com
   - Check logs for errors

3. **Deploy frontend:**
   - Push to main branch (triggers auto-deploy on Vercel)
   - Verify deployment: https://crosswars.xyz
   - Test core functionality

### Post-Release Sanity Checks

After deploying, perform these manual checks:

1. **Frontend loads:** Visit https://crosswars.xyz
2. **User authentication works:** Log in / sign up
3. **Solo play works:** Generate and complete a crossword
4. **Battle mode works:** Create invite, accept on another device, complete battle
5. **API health check:** Visit https://backend-ezw4.onrender.com
6. **Database connectivity:** Verify user actions persist


## Troubleshooting

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
pip install -r requirements.txt
pip install -r requirements-dev.txt
```

**Database connection failures:**
- Verify `.env` file has correct Supabase credentials
- Check Supabase project is active and accessible
- Ensure using service role key (not anon key) for backend

**requirements.txt corruption (Windows):**
```bash
# Delete corrupted file
del requirements.txt

# Create new clean file with UTF-8 encoding
pip freeze | Out-File -Encoding utf8 requirements.txt

# Verify it's clean
type requirements.txt
```

### Frontend Build Issues

**CORS errors in browser:**
- Ensure backend is running on port 8000
- Verify backend CORS settings allow `localhost:5173`
- Check `VITE_API_URL` in `.env` matches backend URL

**npm build failures:**
```bash
cd Frontend
# Windows
Remove-Item node_modules, package-lock.json -Recurse -Force

# Mac/Linux
rm -rf node_modules package-lock.json

# Reinstall
npm install
```

**Environment variables not working:**
- Restart dev server after changing `.env` files
- Ensure frontend variables start with `VITE_`
- Check browser dev tools Console tab for actual values being used

### Crossword Issues

**"No crossword available" error:**
- Generate one: `POST /crossword/generate-daily`
- See [Testing_crosswords.md](Testing_crosswords.md) for advanced testing options

**"Failed to generate crossword" error:**
- Check OpenAI API key in `Backend/.env`
- Verify you have credits in your OpenAI account

**Other crossword issues:**
- See [Testing_crosswords.md](Testing_crosswords.md) for detailed troubleshooting

### Test Failures

**Backend tests fail with "connection error":**
- Ensure backend server is NOT running during tests
- Tests use mocked database, real server interferes

**Frontend tests fail with "module not found":**
- Run `npm install` to ensure all dependencies are installed
- Check that `__mocks__` directory exists with proper mocks

**Import errors in tests:**
- Check that virtual environment is activated (backend)
- Verify all test dependencies are installed (`requirements-dev.txt`)

---

## Related Documentation

- **[setup.md](setup.md)** - Initial project setup, tool installation, repository access
- **[testing.md](testing.md)** - Detailed testing documentation, test structure, coverage
- **[coding-guidelines.md](coding-guidelines.md)** - Code style, naming conventions, best practices

For questions or issues not covered here, contact the development team or open an issue on GitHub.
