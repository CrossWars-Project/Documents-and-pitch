# Testing Guide

## Backend Testing

### Running Tests

**Prerequisites:** 
- Be in the Backend directory with virtual environment active
- Stop the backend server if running (Ctrl+C)

```bash
cd Backend
python -m pytest -q
```

**Test execution options:**
- Run all tests: `python -m pytest`
- Run specific file: `python -m pytest tests/test_invites.py`
- Run specific test: `python -m pytest -k "test_create_invite_auth_user"`
- Verbose output: `python -m pytest -v`
- Quiet mode: `python -m pytest -q`

### How to Add a Test

1. Go to the `tests/` folder and find the relevant test file (or create a new one)
2. Import pytest and any needed modules
3. Create mock objects as needed (users, data, etc.)
4. Define test with `def test_name():`
5. Use `assert` statements to verify expected behavior

**Important:** Tests must be run from the `tests/` folder due to the `conftest.py` file that sets up the testing environment.

## Frontend Testing

### Running Tests

```bash
cd Frontend
npm test
```

**Development options:**
- Watch mode: `npm test -- --watch`
- Coverage report: `npm test -- --coverage`
- Specific file: `npm test ComponentName.test.jsx`
- Specific test: `npm test -- -t "test name"`

### How to Add a Test

1. Create test file next to component: `Component.jsx` → `Component.test.jsx`
2. Import testing utilities: `@testing-library/react`, `@testing-library/user-event`
3. Mock API calls with `jest.mock('../../api')` if needed
4. Wrap components in context providers with mock values if needed
5. Define tests in `describe` blocks:

```javascript
describe('ComponentName', () => {
  test('description of behavior', () => {
    // test code with expect() assertions
  });
});
```

## CI/CD

Both backend and frontend tests run automatically in CircleCI. All tests must pass before code can be merged.