# Changelog

## Unreleased

* Added 8 skills: `commit`, `concise-readme`, `doc-coauthoring`, `frontend-design`, `skill-creator`, `update-changelog`, `uv`, and `webapp-testing`
* Added `uv` extension that redirects `pip`, `pip3`, `poetry`, and `python` to `uv` equivalents, with shims in `intercepted-commands/`
* Added README with project description, getting started instructions, and skills table
* Reworked `project-planner` skill to enforce actual building, sub-milestone structure with stop rules, and granular file-level task specs
* Updated `commit` skill to always update the changelog before committing
