# dotpi
Home folder for pi coding manager

## Getting started

1. Clone repository

   ```bash
   git clone git@github.com:ucyo/dotpi.git .pi/
   ```

2. Start development session

    ```bash
    docker run -it --rm -v $HOME/.pi/:/root/.agents/ -p 8000:8000 ucyo/pi:latest  # mount as global
    docker run -it --rm -v $HOME/.pi/:/workspace/.pi -p 8000:8000 ucyo/pi:latest  # mount in project
    ```

## Skills

| Name                                                 | Description                                                                                                                |
| ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------- |
| [commit](skills/commit/SKILL.md)                     | Read before making git commits                                                                                             |
| [concise-readme](skills/concise-readme/SKILL.md)     | Generate a concise, well-structured README for Docker images, CLI tools, or libraries                                      |
| [doc-coauthoring](skills/doc-coauthoring/SKILL.md)   | Guide users through a structured workflow for co-authoring documentation (proposals, technical specs, decision docs, etc.) |
| [frontend-design](skills/frontend-design/SKILL.md)   | Create distinctive, production-grade frontend interfaces — websites, dashboards, React components, HTML/CSS layouts        |
| [skill-creator](skills/skill-creator/SKILL.md)       | Create new skills, modify and improve existing skills, run evals, benchmark performance, and optimize skill descriptions   |
| [update-changelog](skills/update-changelog/SKILL.md) | Read before updating changelogs                                                                                            |
| [uv](skills/uv/SKILL.md)                             | Use `uv` instead of pip/python/venv for running scripts, adding deps, and managing standalone scripts                      |
| [webapp-testing](skills/webapp-testing/SKILL.md)     | Interact with and test local web applications using Playwright — verify UI, capture screenshots, view browser logs         |
