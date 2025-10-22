# Documentation Automation Toolkit – Setup and Guide

## Overview / Purpose
This repository provides a structured approach to automating the generation and deployment of documentation using Jekyll, with a focus on CI/CD integration through GitHub Actions. It utilizes Ruby and several Jekyll plugins to create documentation sites that can be automatically built and deployed to GitHub Pages. Key features include support for multiple configurations (preview and release), and integration with Docker and GitLab for container deployment.

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
```

## Directory & File Descriptions
- `_config_preview.yml`  # Configuration file for preview builds.
- `_config_release.yml`  # Configuration file for release builds.
- `_config_theme.yml`  # Configuration file for theme settings.
- `_config.yml`  # Main configuration file for Jekyll site.
- `_docs/dockerhub.md`  # Documentation for Docker Hub integration with GitHub Actions.
- `_docs/gitlab-registry.md`  # Guide for blue-green deployment using GitLab Container Registry and Shepherd.
- `_docs/index.md`  # Main index page for documentation site.
- `_docs/my-docs.md`  # Detailed guide on configuring and using the documentation framework.
- `_docs/my-docs/new.md`  # Child page for additional documentation.
- `_docs/n8n.md`  # Guide for deploying n8n in Kubernetes.
- `_docs/new-task.md`  # Practical task related to Docker Swarm and container management.
- `_docs/nginx-blue-green.md`  # Instructions on implementing blue-green deployment strategies with NGINX.
- `_docs/shepherd.md`  # Configuration guide for Shepherd in Docker.
- `_docs/workshop-diagrams.md`  # Diagrams related to practical tasks in Swarm and Docker Hub.
- `_includes/toc.md`  # Table of Contents for documentation pages.
- `.github/workflows/CODEOWNERS`  # Configuration for repository code ownership.
- `.github/workflows/deploy-preview.yml`  # GitHub Actions workflow for deploying preview builds.
- `.github/workflows/release-docs.yml`  # GitHub Actions workflow for manually releasing documentation.
- `Gemfile`  # Defines Ruby gems used for managing dependencies.
- `index.md`  # Homepage for the documentation site.

## Setup / Installation / Deployment
1. **Clone the Repository**
   ```
   git clone https://github.com/<your_username>/my-docs.git
   cd my-docs
   ```

2. **Install Dependencies**
   Ensure you have Ruby and Bundler installed, then run:
   ```
   bundle install
   ```

3. **Build the Documentation Site**
   You can build the documentation for different environments, e.g.:
   ```
   bundle exec jekyll build --config _config.yml,_config_theme.yml,_config_preview.yml
   ```

4. **Configure GitHub Actions for CI/CD**
   - Update the repository settings in GitHub for the GitHub Actions workflows.
   - Ensure `GITHUB_TOKEN` secret is configured in your repository for deployment workflows.

## Usage
- To start a local development server:
   ```
   bundle exec jekyll serve
   ```
- Access the local site via `http://localhost:4000`.

- For deploying preview versions, push to the `develop` branch to trigger the `deploy-preview.yml` workflow or manually trigger the `release-docs.yml` workflow for documentation release.

## Configuration / Environment
- **Environment Variables**: Ensure the following GitHub secrets are set up for successful deployments:
   - `GITHUB_TOKEN`: Token for authentication in GitHub actions.
   - Docker Hub credentials if using Docker workflows.
  
- **Dependencies**: 
   - Ruby >= 3.1
   - Bundler >= 2.4.22
   - Jekyll >= 4.3.2

## Troubleshooting
- If the site does not build, verify that all required gems are installed by running `bundle install`.
- Common issues can arise from incorrect paths in configuration files. Ensure paths are accurate relative to the repository root.

## Automation / CI-CD / Scripts
The repository employs GitHub Actions for CI/CD:
- **CODEOWNERS** file defines repository ownership to manage reviews.
- **Workflow files**:
  - `deploy-preview.yml`: Automatically deploys changes to the GitHub pages when pushing to the `develop` branch.
  - `release-docs.yml`: Manages the release process when triggered manually.

Ensure you have the appropriate permissions configured in the repository settings to enable these workflows to run successfully.

```
```
