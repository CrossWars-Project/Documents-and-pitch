This will contain detailed information on test structure, commands, and seed data
## Testing Automation Infrastructure
- Pytest in the backend repo
    - Why Pytest? Simple syntax, allows creation of reusable fixtures, and integrates nicely with CircleCi.
- Jest in the frontend repo.
    - Why Jest? Based on our research, Jest is the industry standard for Javascript/React testing. It also integrates with CircleCI.


## Backend

### Running Tests Backend

Run tests from the project root with your virtual environment active:
```powershell
python -m pytest -q
```

### If pytest can't find the `app` package:

**Option 1: Use `pytest.ini` (current setup)**
- Adds `pythonpath = .` so `from app...` resolves
- Quickest fix for small repos

**Option 2: Editable install (recommended for development)**
```powershell
python -m pip install -e .
```
- Requires `pyproject.toml` or `setup.cfg`
- Makes package importable for all Python runs

**Option 3: Set PYTHONPATH temporarily**
```powershell
$env:PYTHONPATH = (Get-Location).Path; python -m pytest -q
```

### How to add a test: 
- Go into the tests folder and find the most relevant file for the tests we wish to write or create a new file. 
- Be sure to import pytest. 
- It is often a  good pattern to create a mock user or other mock object we can use in different tests. 
- For tests involving the database, use the mocked db in tests/mocks/mock_supabase.py
- Define each test in the file with `def <test_name>`. 
- Use pytest’s assert statements to ensure expected behavior. 
- We can run a file of tests in our terminal with `pytest -v tests/<test_name>.py` or a specific test with `pytest -v -k <specific_test_name>`. 
- There is a job in CircleCI that enforces that we may not merge any code that does not pass any tests in the test folder.

## Frontend

### Running Tests Frontend

Run tests from the frontend directory:
```bash
npm test
```

**Development options:**
- Watch mode (reruns on file changes): `npm test -- --watch`
- With coverage report: `npm test -- --coverage`
- Specific test file: `npm test ComponentName.test.jsx`
- Specific test: `npm test -- -t "test name"`

### How to add a test:

- Create a test file next to the component you're testing (e.g., `Dashboard.jsx` → `Dashboard.test.jsx`).
- Be sure to import testing utilities from `@testing-library/react` and `@testing-library/user-event`.
- It is often a good pattern to create mock data, mock API responses, or mock context values we can use in different tests.
- For tests involving API calls, mock the functions from `src/api.js` using `jest.mock('../../api')`.
- For tests involving context (from `src/context/`), wrap components in the appropriate context provider with mock values.
- Define each test in the file within a `describe` block: 
```javascript
  describe('ComponentName', () => {
    test('description of behavior', () => {
      // test code
    });
  });
```
- Use `expect()` assertions to ensure expected behavior (e.g., `expect(element).toBeInTheDocument()`).
- We can run a file of tests in our terminal with `npm test ComponentName.test.jsx` or a specific test with `npm test -- -t "specific test name"`.
- There is a job in CircleCI that enforces that we may not merge any code that does not pass all tests.