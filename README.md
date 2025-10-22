```
# Documentation Automation Repository – CI/CD and Container Management Guide

## Overview / Purpose
This repository serves as a comprehensive guide for automating documentation and deploying applications using CI/CD practices with GitHub Actions, Docker, and Jekyll. It emphasizes best practices in container orchestration with Docker Swarm and provides a platform for developing, documenting, and deploying services. The key technologies utilized in this project include:
- **Jekyll**: For generating static websites.
- **Docker**: For containerizing applications and services.
- **GitHub Actions**: For automated CI/CD workflows.

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
- `_config_preview.yml`  # Jekyll config for preview environment
- `_config_release.yml`  # Jekyll config for release builds
- `_config_theme.yml`    # Theme configuration for Jekyll
- `_config.yml`          # Main Jekyll configuration file
- `_docs/dockerhub.md`   # Documentation on Docker Hub Cloud Build
- `_docs/gitlab-registry.md`  # Guide to Blue/Green deployment with GitLab
- `_docs/index.md`       # Index of documentation topics
- `_docs/my-docs.md`     # CI/CD task details
- `_docs/my-docs/new.md` # Sub-page of my-docs for expanded info
- `_docs/n8n.md`         # Guide to deploying n8n in Kubernetes
- `_docs/new-task.md`    # Task on container orchestration
- `_docs/nginx-blue-green.md`  # Blue-Green strategy documentation
- `_docs/shepherd.md`     # Instructions for Shepherd DockerHub integration
- `_docs/workshop-diagrams.md`  # Workshop diagrams for practical tasks
- `_includes/toc.md`     # Table of contents for documentation
- `.github/workflows/CODEOWNERS`  # Defines code owner permissions
- `.github/workflows/deploy-preview.yml`  # CI workflow for preview deployment
- `.github/workflows/release-docs.yml`  # CI workflow for releasing documentation
- `Gemfile`              # Ruby dependencies for the Jekyll site
- `index.md`            # Main landing page for the documentation site
- `readme.md`           # Initial README file with basic info

## Setup / Installation / Deployment
To set up this project:
1. **Clone the Repository**:
   ```bash
   git clone https://github.com/yourusername/your-repo.git
   cd your-repo
   ```
2. **Install Dependencies**:
   Ensure you have Ruby and Bundler installed, then run:
   ```bash
   bundle install
   ```
3. **Run the Jekyll Server**:
   To preview the site locally:
   ```bash
   bundle exec jekyll serve
   ```

4. **Configure GitHub Actions**:
   Set up the repository on GitHub with proper access tokens and permissions for actions to work.

5. **Deploying**:
   - For deployment, push to the `develop` branch to trigger the deploy-preview workflow.
   - For a release build, use the manual release document workflow and follow the prompts.

## Usage
To utilize the project, navigate to the local server URL displayed in the terminal (default is `http://localhost:4000`). You can access different documentation pages dedicated to various tasks and setups.

## Configuration / Environment
The following environment variables are considered:
- **DOCKER_USER**: Your Docker Hub username. 
- **DOCKER_PAT**: Your Docker Hub personal access token.
- GitHub Secrets should include the necessary tokens for CI/CD operations.

Dependencies include:
- Ruby 3.1
- Bundler 2.4.22
- Jekyll 4.3.2 and associated plugins specified in the Gemfile.

## Troubleshooting
Common issues may include:
- **"Error response from daemon: could not choose an IP address..."**: Ensure that the correct IP address is specified during Docker Swarm setup.
- **Build errors**: Ensure all dependencies are correctly installed and check your Gemfile for compatibility.

## Automation / CI-CD / Scripts
The repository implements two GitHub Actions workflows:
1. **deploy-preview.yml**: Triggers on `push` to `develop` for creating a preview deployment of the documentation site.
2. **release-docs.yml**: Can be manually triggered to build and deploy the documentation site upon version change.

These workflows automate the process using Ruby, Jekyll, and GitHub's built-in CI features, thus allowing for seamless documentation updates and deployments.

```
