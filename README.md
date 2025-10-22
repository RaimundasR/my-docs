# CI/CD Documentation and Automation Toolkit – Deployment and Configuration Guide

## Overview
This repository serves as a comprehensive toolkit for Continuous Integration and Continuous Deployment (CI/CD) workflows utilizing Jekyll for documentation and various automation tools such as GitHub Actions. The primary goal is to facilitate the automated building, releasing, and deployment of documentation with Docker and Kubernetes integration strategies.

### Key Technologies
- **Jekyll:** Static site generator for documentation
- **GitHub Actions:** CI/CD automation framework
- **Docker:** Containerization tool for building and deploying applications
- **Kubernetes:** Container orchestration platform
- **Shepherd:** Deployment manager for Docker Swarm
- **NGINX:** Reverse proxy server for Docker containers

## File Tree
```
.
├── _config_preview.yml               # Jekyll preview configuration
├── _config_release.yml               # Jekyll release configuration
├── _config_theme.yml                 # Jekyll theme configuration
├── _config.yml                       # Jekyll main configuration
├── _docs/dockerhub.md                # Documentation for Docker Hub CI/CD integration
├── _docs/gitlab-registry.md          # Blue/Green deployment using GitLab
├── _docs/index.md                    # Index file for documentation navigation
├── _docs/my-docs.md                  # Main documentation file with tasks
├── _docs/my-docs/new.md              # Child page under main documentation
├── _docs/n8n.md                      # n8n deployment in Kubernetes guide
├── _docs/new-task.md                 # Practical task on container management
├── _docs/nginx-blue-green.md          # Blue-Green deployment strategy with NGINX
├── _docs/shepherd.md                 # Shepherd DockerHub authentication setup
├── _docs/workshop-diagrams.md        # Diagrams for practical tasks with Docker
├── _includes/toc.md                  # Table of contents for included documentation
├── .github/workflows/CODEOWNERS      # GitHub CODEOWNERS file for PR reviews
├── .github/workflows/deploy-preview.yml # GitHub Actions for deploying preview documentation
├── .github/workflows/release-docs.yml # GitHub Actions for manual documentation release
├── Gemfile                            # Ruby dependency manager for Jekyll
├── index.md                           # Home page of Jekyll site
├── readme.md                          # This README file for project overview
```

## Architecture & Structure
The repository is organized as follows:

- **Configuration Files**:  
  - `_config.yml`: Main configuration for Jekyll.
  - `_config_theme.yml`: Configuration for the theme used.
  - `_config_preview.yml` & `_config_release.yml`: Specific configurations for preview and release builds.

- **Documentation Files**:  
  - `_docs/`: Contains various markdown files for different CI/CD tasks and guides including integrations with Docker Hub and GitLab.

- **Automation Workflows**:  
  - `.github/workflows/`: Contains GitHub Actions workflows for code ownership, deploying previews, and manual documentation releases.

- **Dependencies**:  
  - `Gemfile`: Specifies the Ruby gems required to run the Jekyll site efficiently.

## Deployment & Setup
### Prerequisites
- Install Ruby and Bundler on your system.
- Set up GitHub repository with Pages enabled.

### Installation Steps
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/your_username/my-docs.git
   cd my-docs
   ```

2. **Install Dependencies**:
   ```bash
   bundle install
   ```

3. **Build the Site**:
   To build for preview:
   ```bash
   bundle exec jekyll build --config _config.yml,_config_theme.yml,_config_preview.yml -d _site
   ```

4. **Deploy Previews via GitHub Actions**:
   Push changes to `develop` branch to trigger preview deployment.

5. **Manual Documentation Release**:
   Use the `workflow_dispatch` trigger in GitHub Actions to manually release updates.

## Configuration & Environment
### Required Environment Variables
- **GITHUB_TOKEN**: Access token required for GitHub Actions to publish to Pages.

### Configuration Parameters
- **baseurl**: Specify the base URL for the site in `_config_release.yml`.

### Dependencies
- Ruby versions specified in the `Gemfile`.
- Jekyll plugins as per the `Gemfile`.

## Usage
- Run the development server:
  ```bash
  bundle exec jekyll serve
  ```
- Access the documentation locally at `http://localhost:4000`.

## Troubleshooting
- If you encounter build errors:
  - Ensure all dependencies are installed correctly.
  - Check the `.github/workflows/*` for errors in GitHub CI workflows.

- If the Jekyll site does not build:
  - Validate YAML syntax in `_config.yml` and related files using a YAML linter.

## Automation & CI/CD
- **GitHub Actions Workflows**:
  - **deploy-preview.yml**: Automates building and deploying preview versions on pushes to the `develop` branch.
  - **release-docs.yml**: Facilitates manual triggering of a release for documentation.

- **Helm Charts**: Not directly present but can be inferred as a design pattern for Kubernetes deployments illustrated in `_docs/n8n.md`.

This README aims to provide a comprehensive overview of the project structure and operations related to CI/CD in the context of documentation deployment and automation. Utilize it to familiarize yourself with the setup and usage of this toolkit.