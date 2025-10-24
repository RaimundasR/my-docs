# Documentation Automation and CI/CD Framework – Setup and Usage Guide

## Overview / Purpose
This repository serves as a robust framework for automating documentation deployment using GitHub Actions and Jekyll. It is tailored for generating, publishing, and maintaining documentation sites with version control. Utilizing technologies such as Jekyll for static site generation, GitHub Actions for CI/CD workflows, and Docker for containerization, this project simplifies the documentation process while ensuring easy collaboration and version tracking.

## File Tree
```
.
├── _docs
│   ├── my-docs
│   │   └── new.md
│   ├── dockerhub.md
│   ├── gitlab-registry.md
│   ├── index.md
│   ├── my-docs.md
│   ├── n8n.md
│   ├── new-task.md
│   ├── nginx-blue-green.md
│   ├── shepherd.md
│   └── workshop-diagrams.md
├── _includes
│   └── toc.md
├── github
│   └── workflows
│       ├── CODEOWNERS
│       ├── deploy-preview.yml
│       └── release-docs.yml
├── _config_preview.yml
├── _config_release.yml
├── _config_theme.yml
├── _config.yml
├── Gemfile
└── index.md
```

## Directory & File Descriptions
- `_docs/`: Directory containing all documentation files.
  - `my-docs/`: Contains child documentation files for specific projects.
  - `dockerhub.md`: Guide for Docker Hub automation using GitHub Actions.
  - `gitlab-registry.md`: Instructions for Blue/Green deployment with GitLab.
  - `index.md`: Overview and navigation for the documentation site.
  - `my-docs.md`: Main documentation section detailing Jekyll automation.
  - `n8n.md`: Instructions for deploying n8n in Kubernetes.
  - `new-task.md`: Practical task guidelines for Docker Swarm.
  - `nginx-blue-green.md`: Information on implementing Blue-Green strategy with NGINX.
  - `shepherd.md`: Overview of using Shepherd with Docker Swarm for auto-updating services.
  - `workshop-diagrams.md`: ASCII diagrams for practical tasks involving Docker and Swarm.

- `_includes/`: Contains reusable components for documentation.
  - `toc.md`: Table of contents used for navigation.

- `github/workflows/`: Contains GitHub Actions workflow files.
  - `CODEOWNERS`: Specifies code reviewers for repository changes.
  - `deploy-preview.yml`: Configuration for deploying documentation previews on the develop branch.
  - `release-docs.yml`: Manual workflow for releasing documentation versions.

- `_config_preview.yml`: Configuration for preview builds of the documentation.
- `_config_release.yml`: Configuration parameters for released documentation versions.
- `_config_theme.yml`: Theme configuration for Jekyll documentation site.
- `_config.yml`: Main site configuration for Jekyll.
- `Gemfile`: Lists Ruby dependencies needed for the Jekyll site.
- `index.md`: Serves as the entry point for navigating the documentation site.

## Setup / Installation / Deployment
1. **Clone the repository**:
   ```
   git clone https://github.com/yourusername/repository.git
   cd repository
   ```

2. **Install Ruby and Bundler**:
   Ensure you have Ruby (3.1 recommended) and Bundler installed. You can install them via:
   ```
   sudo apt-get install ruby-full
   gem install bundler
   ```

3. **Install dependencies**:
   From the root of the repository, run:
   ```
   bundle install
   ```

4. **Run Jekyll locally**:
   You can serve the documentation locally by executing:
   ```
   bundle exec jekyll serve
   ```
   Access the site at `http://localhost:4000`.

5. **Deploy to GitHub Pages**:
   Commit your changes and push them to the `develop` branch to trigger CI/CD workflows for deployment.

## Usage
- To create new documentation pages, add Markdown files in the `_docs/` directory following the structure used in existing files.
- Use GitHub Actions workflows for continuous deployment and versioning. Trigger them by pushing changes to the appropriate branches, as defined in the workflow files.

## Configuration / Environment
- **Dependencies**: Ensure Ruby and Bundler are installed.
- **Environment Variables**:
  - `GITHUB_TOKEN`: A GitHub token is required for deploying updates to GitHub Pages and must be set in GitHub Secrets.
- **Configuration Files**:
  - `_config.yml`: Basic site settings (title, plugins, etc.).
  - `_config_preview.yml` and `_config_release.yml`: Specify the base URL for different environments.

## Troubleshooting
- If the site does not build:
  - Check for errors in the local terminal output for Ruby dependency issues.
  - Ensure that all files follow the proper Jekyll structure and syntax.
  
- CI/CD jobs fail:
  - Verify that the GitHub Secrets are correctly configured, especially for the GITHUB_TOKEN.

## Automation / CI-CD / Scripts
- **GitHub Actions**: 
  - The `deploy-preview.yml` is triggered on pushes to the `develop` branch, automatically building and deploying the preview of the documentation.
  - The `release-docs.yml` allows for manual triggering to prepare a specific versioned release, publishing to GitHub Pages.
- These workflows ensure that changes in documentation are automatically reflected on the live site without manual intervention, streamlining the deployment process.

```
```
