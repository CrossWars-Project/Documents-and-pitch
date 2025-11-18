# Frontend Workflow
1. Start from the latest main branch
```bash
git checkout main
git pull origin main
```
2. Check out a branch using checkout or switch
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
> When applicable, run the backend to fully walk through and manually test the frontend on the local host
5. Push branch to origin
```bash
git push origin <branch-name>
```
6. Make Pull Request
7. Automated testing/ code review
8. Merge to Main