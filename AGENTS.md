- After each request, add an entry to prompts.md: what I asked, what you changed, and whether it worked.

## Project layout

This is a single-file web app project.

- `AGENTS.md` contains project instructions and stays at the top level.
- `spec.md` contains the app requirements and stays at the top level.
- `prompts.md` contains the prompt log and stays at the top level.
- `index.html` contains the app and stays at the top level so GitHub Pages can serve it.
- `00_planning/` is for notes, sketches, and feature ideas.
- `01_data/` is for data files, such as USGS downloads.
- `02_validation/` is for hand calculations and test cases.
- `03_output/` is for screenshots and exported results.

## File-placement rule

Save each new file in the folder that matches its purpose. Keep project requirements and app source in the top-level files listed above; put supporting project files only in the matching numbered folder.