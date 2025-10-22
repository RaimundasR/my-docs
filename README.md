# Documentation Automation for My Docs Site – CI/CD and Deployment Overview

## Overview / Purpose
This repository provides a structured approach to managing, building, and deploying documentation using Jekyll in conjunction with GitHub Actions for CI/CD automation. It leverages various features, including blue-green deployments and containerization within a Docker Swarm environment, to enhance deployment strategies. Key technologies utilized include Jekyll, Ruby, Docker, GitHub Actions, and Helm for Kubernetes operations.

## File Tree
```
.
├── _config_preview.yml
├── _config_release.yml
├── _config_theme.yml
├── _config.yml
├── _docs/dockerhub.md
├── _docs/gitlab-registry.md
├── _docs/index.md
├── _docs/my-docs.md
├── _docs/my-docs/new.md
├── _docs/n8n.md
├── _docs/new-task.md
├── _docs/nginx-blue-green.md
├── _docs/shepherd.md
├── _docs/workshop-diagrams.md
├── _includes/toc.md
├── .github/workflows/CODEOWNERS
├── .github/workflows/deploy-preview.yml
├── .github/workflows/release-docs.yml
├── Gemfile
├── index.md
├── readme.md
```

## Directory & File Descriptions
- `_config_preview.yml`     # Jekyll configuration for preview environment.
- `_config_release.yml`     # Jekyll configuration for release environment.
- `_config_theme.yml`       # Theme configuration for Jekyll.
- `_config.yml`             # Main Jekyll configuration file.
- `_docs/dockerhub.md`      # Documentation for Docker Hub setup using GitHub Actions.
- `_docs/gitlab-registry.md` # Documentation for blue-green deployment using GitLab Container Registry.
- `_docs/index.md`          # Main index page for documentation site.
- `_docs/my-docs.md`        # Content page for CI/CD tasks and automation instruction.
- `_docs/my-docs/new.md`    # Child page under my-docs for additional documentation.
- `_docs/n8n.md`            # Guide for deploying n8n in Kubernetes.
- `_docs/new-task.md`       # Practical task document related to Docker Swarm.
- `_docs/nginx-blue-green.md` # Documentation for implementing blue-green deployment with NGINX.
- `_docs/shepherd.md`       # Guide for using Shepherd with Docker Swarm.
- `_docs/workshop-diagrams.md` # Diagrams related to practical work using Docker and Swarm.
- `_includes/toc.md`        # Includes table of contents for pages.
- `.github/workflows/CODEOWNERS` # Specifies code ownership rules in the repository.
- `.github/workflows/deploy-preview.yml` # GitHub Actions workflow for deploying previews.
- `.github/workflows/release-docs.yml` # GitHub Actions workflow for manual documentation releases.
- `Gemfile`                 # Ruby dependencies for Jekyll and associated gems.
- `index.md`                # Main landing page for the documentation site.
- `readme.md`               # Basic readme file (to be expanded).

## Setup / Installation / Deployment
1. **Clone the repository**:
   ```
   git clone https://github.com/<your-username>/my-docs.git
   cd my-docs
   ```

2. **Install dependencies** using Bundler:
   ```
   bundle install
   ```

3. **Configure your environment**:
   - Modify `_config.yml`, `_config_theme.yml`, and other configuration files as necessary to fit your requirements.

4. **Run Jekyll locally** to test:
   ```
   bundle exec jekyll serve
   ```

5. **Deploy to GitHub Pages**:
   - Changes pushed to `main` trigger a deployment via GitHub Actions as defined in `.github/workflows/deploy-preview.yml` and `.github/workflows/release-docs.yml`.

## Usage
- Navigate to the homepage of your Jekyll site once Jekyll is running locally. 
- Use GitHub Actions to manage CI/CD for deployments automatically when pushing changes to certain branches.

## Configuration / Environment
- Ensure the following dependencies are included in your environment:
  - Ruby version: `3.1`
  - Bundler version: `2.4.22`
  - Jekyll version: `4.3.2`
  
- Environment variables are managed via GitHub Actions and can be set in repository secrets:
  - `DOCKER_USER`: Your Docker Hub username.
  - `DOCKER_PAT`: Your Docker Hub Personal Access Token.

## Troubleshooting
- **Issue: Jekyll build fails**:
  - Ensure all dependencies are installed correctly. Rerun `bundle install`.

- **Issue: Deployment does not trigger**:
  - Check GitHub Action logs and ensure the correct branch triggers are set in your workflows.

## Automation / CI-CD / Scripts
- The workflow defined in `.github/workflows/deploy-preview.yml` builds the Jekyll site on pushes to the `develop` branch and deploys it to the `gh-pages` branch for previews.
- The manual deployment process is managed via `.github/workflows/release-docs.yml`, which allows for versioned releases to be published on GitHub Pages.

With these guidelines, contributors and users can effectively work with the repository and utilize the documentation automation processes established within.
