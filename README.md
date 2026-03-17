# dotpi

Pi coding agent configuration — skills and settings for use with `ucyo/pi` Docker images, mountable globally or per project.

## Getting Started

- **Mount globally and run:**
    ```bash
    docker run -it --rm -v $HOME/.pi/:/root/.agents/ -p 8000:8000 ucyo/pi:latest
    ```

- **Mount per project and run:**
    ```bash
    docker run -it --rm -v $HOME/.pi/:/workspace/.pi -p 8000:8000 ucyo/pi:latest
    ```

- **Clone this config:**
    ```bash
    git clone git@github.com:ucyo/dotpi.git .pi/
    ```

## Skills

| Name | Description |
|------|-------------|
| [commit](skills/commit/SKILL.md) | Read before making git commits |
| [concise-readme](skills/concise-readme/SKILL.md) | Generate a concise, well-structured README for Docker images, CLI tools, or libraries |
| [doc-coauthoring](skills/doc-coauthoring/SKILL.md) | Guide users through a structured workflow for co-authoring documentation |
| [frontend-design](skills/frontend-design/SKILL.md) | Create distinctive, production-grade frontend interfaces |
| [skill-creator](skills/skill-creator/SKILL.md) | Create, improve, and benchmark skills |
| [update-changelog](skills/update-changelog/SKILL.md) | Read before updating changelogs |
| [uv](skills/uv/SKILL.md) | Use `uv` instead of pip/python/venv |
| [webapp-testing](skills/webapp-testing/SKILL.md) | Test local web apps with Playwright |
