# Backend Workflow
1. Start from the latest main branch
```bash
git checkout main
git pull origin main
```
2. Check out a branch using checkout or switch
```bash
git checkout -b <feature-or-fix>/<description>
git switch -c <feature-or-fix>/<description>
```
2. Activate the virtual environment:
   - Windows: `.\.venv313\Scripts\Activate.ps1`
   - Mac:`source .venv313/bin/activate`

> If you are getting import errors, you may need to reinstall requirements. Refer back to the setup instructions above for installs.
>
> or if that doesn't work, try: `pip install fastapi uvicorn supabase python-dotenv pytest`

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
6. End dev session, aka leave venv
```bash
deactivate
```
7. Open Pull Request
8. Automated testing/ code review
9. Merge to main