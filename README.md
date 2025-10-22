# My Documentation Repository

## Overview
This repository is designed for managing project documentation with automation setups for deployment using GitHub Actions. It leverages Jekyll for static site generation and supports blue-green deployment strategies using Docker.

## File Tree
```
.
├── _config_preview.yml              # Configuration for preview deployment
├── _config_release.yml              # Configuration for release deployment
├── _config_theme.yml                # Theme configuration for Jekyll
├── _config.yml                      # Main Jekyll configuration file
├── _docs/dockerhub.md               # Documentation for Docker Hub Cloud Build with GitHub Actions
├── _docs/gitlab-registry.md         # Blue/Green Deploy documentation using GitLab Container Registry
├── _docs/index.md                   # Index page for the documentation site
├── _docs/my-docs.md                 # Documentation for CI/CD tasks
├── _docs/my-docs/new.md             # Sub-page for new documentation under my-docs
├── _docs/n8n.md                     # Deployment guide for n8n in Kubernetes
├── _docs/new-task.md                # Practical task for creating and managing Docker Swarm
├── _docs/nginx-blue-green.md        # Blue-Green strategy documentation using Shepherd and NGINX
├── _docs/shepherd.md                # Documentation for Shepherd DockerHub authentication with Swarm
├── _docs/workshop-diagrams.md       # Workshop diagrams for practical tasks
├── _includes/toc.md                 # Table of contents include for documentation pages
├── .github/workflows/CODEOWNERS     # Code ownership configuration for repository
├── .github/workflows/deploy-preview.yml # GitHub Actions workflow for deploying preview builds
├── .github/workflows/release-docs.yml # GitHub Actions workflow for manual documentation release
├── Gemfile                           # Ruby dependencies for Jekyll and plugins
├── index.md                          # Home page for the documentation site
├── readme.md                         # Project README file
```

## Directory & File Descriptions
- **_config_preview.yml**: Configuration used during preview builds.
- **_config_release.yml**: Configuration used for production release builds.
- **_config_theme.yml**: Sets the theme for the Jekyll website.
- **_config.yml**: Main configuration file for Jekyll which specifies settings for the site.
- **_docs/**: Contains various documentation pages in markdown format.
- **.github/workflows/**: Contains GitHub Actions workflows for automating deployment and documentation releases.
- **Gemfile**: Lists necessary Ruby gems for Jekyll, Bundler, and other dependencies.
- **index.md**: Main entry point for the documentation website linking to various documents.
- **readme.md**: Contains brief instructions and repository information.

## Deployment & Setup
1. **Prerquisites**: Ensure that Ruby and Jekyll are installed on your machine. You may also need Docker for local development.
2. **Clone the Repository**:
   ```bash
   git clone https://github.com/your_username/my-docs.git
   cd my-docs
   ```
3. **Install Dependencies**:
   ```bash
   bundle install
   ```
4. **Build the Site**: 
   ```bash
   bundle exec jekyll build
   ```

5. **Serve Locally**: 
   ```bash
   bundle exec jekyll serve
   ```
   Access the site at `http://localhost:4000`.

## Configuration & Environment
- Environment variables for the deployment may be set in the GitHub Secrets under repository settings. Required variables include:
  - `GITHUB_TOKEN`: Required for deploying to GitHub Pages.
  - Docker Hub credentials to push images if using Docker-related workflows.

## Services
- The repository uses:
  - **Jekyll**: For static site generation.
  - **GitHub Actions**: For CI/CD, deploying preview versions and managing documentation releases.

## Troubleshooting
- **Common Issues**:
  - If the site is not building or rendering correctly, make sure all dependencies in the `Gemfile` are installed properly.
  - Check workflow logs in GitHub Actions for deployment issues.
  - For local serving issues, ensure you are in the correct directory and that there are no port conflicts.

Feel free to reach out for any further assistance or clarification on using this repository!