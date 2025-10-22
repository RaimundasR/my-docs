# Documentation Automation Repository – Setup and Configuration Guide

## Overview / Purpose
This repository is designed to automate the process of creating, managing, and deploying documentation websites using Jekyll. Leveraging continuous integration and deployment (CI/CD) with GitHub Actions, this project facilitates the building of documentation for software projects, enabling easy versioning and deployment. The primary technologies used include Jekyll for static site generation, Docker for containerization, and Helm for Kubernetes deployment.

## File Tree
```
.
├── _config_preview.yml
├── _config_release.yml
├── _config_theme.yml
├── _config.yml
├── _docs/dockerhub.md  # Docker Hub Cloud Build documentation
├── _docs/gitlab-registry.md  # Blue/Green Deploy using GitLab Container Registry
├── _docs/index.md  # Landing page for the documentation site
├── _docs/my-docs.md  # Primary documentation guide
├── _docs/my-docs/new.md  # Child page of the primary documentation
├── _docs/n8n.md  # Deployment guide for n8n in Kubernetes
├── _docs/new-task.md  # Practical task for creating a Docker Swarm cluster
├── _docs/nginx-blue-green.md  # Blue-Green deployment strategy with NGINX and Docker Swarm
├── _docs/shepherd.md  # Documentation for using Shepherd with Docker Swarm
├── _docs/workshop-diagrams.md  # ASCII diagrams for practical tasks
├── _includes/toc.md  # Table of contents for documentation pages
├── .github/workflows/CODEOWNERS  # Ownership rules for GitHub workflows
├── .github/workflows/deploy-preview.yml  # Workflow for deploying preview documentation
├── .github/workflows/release-docs.yml  # Manual documentation release workflow
├── Gemfile  # Dependencies for Jekyll and plugins
├── index.md  # Main page of the site
```

## Directory & File Descriptions
- `_config_preview.yml`  # Configuration for preview deployment
- `_config_release.yml`  # Configuration for release deployment
- `_config_theme.yml`  # Theme configuration for the Jekyll site
- `_config.yml`  # Primary configuration file for Jekyll site
- `_docs/dockerhub.md`  # Docker Hub Cloud Build documentation
- `_docs/gitlab-registry.md`  # Guide for Blue/Green deploy using GitLab
- `_docs/index.md`  # Landing page for the documentation site
- `_docs/my-docs.md`  # Primary documentation guide
- `_docs/my-docs/new.md`  # Child page of the primary documentation
- `_docs/n8n.md`  # Deployment guide for n8n in Kubernetes
- `_docs/new-task.md`  # Practical task for Docker Swarm cluster creation
- `_docs/nginx-blue-green.md`  # Blue-Green deployment strategy guide
- `_docs/shepherd.md`  # Documentation for using Shepherd
- `_docs/workshop-diagrams.md`  # ASCII diagrams for practical guidance
- `_includes/toc.md`  # Table of contents for all documentation pages
- `.github/workflows/CODEOWNERS`  # Defines repository management roles
- `.github/workflows/deploy-preview.yml`  # CI workflow for deploying documentation previews
- `.github/workflows/release-docs.yml`  # Manual workflow for releasing documentation
- `Gemfile`  # Specifies dependencies for the Jekyll site
- `index.md`  # Main entry point of the documentation

## Setup / Installation / Deployment
1. **Clone the Repository**: 
   ```
   git clone https://github.com/<your_username>/my-docs.git
   cd my-docs
   ```

2. **Install Dependencies**:
   Ensure you have Ruby and Bundler installed. Then run:
   ```
   bundle install
   ```

3. **Build the Jekyll Site**:
   To build the site locally, use:
   ```
   bundle exec jekyll serve
   ```

4. **Setup GitHub Secrets**:
   For CI/CD to work, add necessary GitHub secrets under repository settings. Example secrets might include `DOCKER_USER` and `DOCKER_PAT`.

5. **Deploy the Site**: Push changes to the `develop` branch to trigger the deployment workflows, or manually run the workflow for releases.

## Usage
- To serve the documentation locally, execute the command:
  ```
  bundle exec jekyll serve
  ```
  The documentation will be available at `http://localhost:4000`.

- For deploying latest documentation to GitHub Pages, use the CI/CD workflow defined in `.github/workflows/release-docs.yml`.

## Configuration / Environment
### Dependencies
- Ruby: 3.1
- Jekyll: 4.3.2
- Bundler: 2.4.22
- GitHub Actions for CI/CD
  
### Configuration Files
- `_config.yml`: Main configuration for Jekyll site.
- `_config_preview.yml`: Configuration specific for preview deployments.
- `_config_release.yml`: Configuration for release versioning.

## Troubleshooting
- **Local Server Issues**: If Jekyll fails to serve the site, verify that all dependencies are installed correctly.
- **GitHub Actions Failures**: Check repository settings for missing secrets or incorrect configurations.

## Automation / CI-CD / Scripts
### GitHub Workflows
- **Deploy Preview**: The `deploy-preview.yml` workflow automatically builds and deploys the documentation whenever changes are pushed to the `develop` branch.
- **Release Docs**: The `release-docs.yml` allows for manual releases, prompting for version input to manage documentation versions effectively.

This project allows for streamlined documentation management, providing a straightforward framework for continuous integration and deployment of documentation websites. Follow the provided guidelines to effectively set up, configure, and deploy your Jekyll-based documentation easily.
