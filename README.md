# Documentation for My Docs Automation System

## Overview
The **My Docs Automation System** repository is designed to facilitate automated documentation generation and publishing using Jekyll, leveraging GitHub Actions for CI/CD workflows. It enables users to create, manage, and deploy documentation efficiently, with additional functionalities for Docker integration and blue-green deployment strategies.

## File Tree
```
.
├── _config_preview.yml               # Configuration for preview environment
├── _config_release.yml               # Configuration for release environment
├── _config_theme.yml                 # Configuration for theme settings
├── _config.yml                       # Main Jekyll configuration file
├── _docs/dockerhub.md                # Documentation for Docker Hub integration
├── _docs/gitlab-registry.md          # Documentation for GitLab registry deployments
├── _docs/index.md                    # Index page for documentation site
├── _docs/my-docs.md                  # Documentation structure for "My Docs"
├── _docs/my-docs/new.md              # Child documentation page
├── _docs/n8n.md                      # n8n deployment guide on Kubernetes
├── _docs/new-task.md                 # Task for implementing containerization
├── _docs/nginx-blue-green.md          # Guide for blue-green deployment with NGINX
├── _docs/shepherd.md                  # Documentation for Shepherd configuration in Docker Swarm
├── _docs/workshop-diagrams.md         # ASCII diagrams for workshops
├── _includes/toc.md                  # Table of contents include file
├── .github/workflows/CODEOWNERS      # Specifies code review requirements
├── .github/workflows/deploy-preview.yml # GitHub Action for preview deployments
├── .github/workflows/release-docs.yml # GitHub Action for releasing documentation
├── Gemfile                            # Ruby gem dependencies for Jekyll
├── index.md                           # Main landing page for the documentation
├── readme.md                          # Project README file
```

## Architecture & Structure
The repository is structured to support a typical Jekyll site integrated with version control and CI/CD. Key components include:

- **Configuration Files**: `_config.yml`, `_config_preview.yml`, `_config_release.yml` define settings for the Jekyll site and publishing behavior for different environments.
- **Documentation**: The `_docs/` folder contains markdown files that serve as the content for the documentation website.
- **GitHub Workflows**: Located under `.github/workflows/`, these YAML files automate the deployment process for documentation and manage code ownership.
- **Gemfile**: Lists the required Ruby gems for building the Jekyll site, ensuring all necessary components are installed.

## Deployment & Setup
1. **Clone the Repository**:  
   ```bash
   git clone https://github.com/<your_github_username>/my-docs.git
   cd my-docs
   ```

2. **Install Dependencies**:  
   Ensure you have Ruby installed, then run:  
   ```bash
   bundle install
   ```

3. **Build the Site**:  
   To build the documentation site locally:  
   ```bash
   bundle exec jekyll build
   ```

4. **Deploy to GitHub Pages**:
   - For preview deployments, push changes to the `develop` branch; this will trigger the `deploy-preview.yml` workflow.
   - For a manual release, trigger the `release-docs.yml` workflow and provide a version number.

## Configuration & Environment
### Required Environment Variables
- **GITHUB_TOKEN**: Required for GitHub Actions to authenticate and interact with the repository.

### Configuration Files
- `_config.yml`: Main site configuration.
- `_config_preview.yml`: Used for preview builds.
- `_config_release.yml`: Used when releasing documentation, allows setting a base URL based on the version.

### Dependencies
- Jekyll: Version `4.3.2`
- Bundler: Version `2.4.22`
- Other gems specified in `Gemfile`.

## Troubleshooting
- **Common Issues**:
  - If the site fails to build, check the output logs for missing dependencies.
  - Ensure that all environment variables and GitHub secrets are correctly configured.

- **Recovery Steps**:
  - Run `bundle install` to ensure all gems are correctly installed.
  - If the GitHub Actions workflows are failing, check that the YAML files are correctly formatted and the necessary permissions are granted.

## Scripts & Automation
There are several automation scripts included for seamless CI/CD processes:
- **GitHub Actions Workflows**: 
  - `deploy-preview.yml` automates the deployment of the preview version of the documentation when changes are pushed to the `develop` branch.
  - `release-docs.yml` allows for manual release of documentation based on specified version tags.

This automation enhances the workflow, allowing for continuous integration and delivery of documentation updates efficiently.