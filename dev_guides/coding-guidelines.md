# Styling Guide and Continuous Integration

This section is to clarify formatting rules and CI guidelines

## Style Guides
- [Python](https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html)
    - Follow the official python style guide
    - Running `make format` will automatically use black to reformat all files to fit the style guide
        > Window users should pay special attention to [Setup](#setup) and ensure they have installed `make` for windows.
- [JavaScript](https://airbnb.io/javascript/react/)
    - Popular style guide and allows ESLint configuration to enforce style.
## Continuous integration
- This project uses circleci for continuous integration.
- circleci is connected to GitHub, it automatically runs its tests and permits PRs from being merged unless they pass. 
- circleci is testing for any missing dependencies, linting and type checking, code passes all unit tests. 
> Remember to always `make format` before attempting the PR to avoid unnecessary errors.
