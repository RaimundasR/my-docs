# Documentation for My Documentation Automation Tool

## Overview
This repository is designed to automate the generation and deployment of documentation using Jekyll and GitHub Actions. It serves as a template for creating Jekyll-based documentation sites with the capability to handle versioning and deployment through CI/CD practices.

## File Tree
```
.
├── _config_preview.yml              # Configuration for preview deployment
├── _config_release.yml              # Configuration for release version deployment
├── _config_theme.yml                # Configuration specific to the Just the Docs theme
├── _config.yml                       # General configuration for the Jekyll site
├── _docs/dockerhub.md                # Documentation on Docker Hub integration with GitHub Actions
├── _docs/gitlab-registry.md          # Blue/Green deployment using GitLab Container Registry and Shepherd
├── _docs/index.md                    # Index page for the documentation site
├── _docs/my-docs.md                  # Documentation outline and automation instructions
├── _docs/my-docs/new.md              # Child page under my-docs for new topics
├── _docs/n8n.md                      # Deployment guide for n8n in Kubernetes
├── _docs/new-task.md                 # Practical task documentation for Docker Swarm
├── _docs/nginx-blue-green.md         # Documentation for Blue-Green strategy in Docker Swarm
├── _docs/shepherd.md                 # Shepherd DockerHub authentication setup with Swarm
├── _docs/workshop-diagrams.md        # Visual diagrams for practical workshops
├── _includes/toc.md                  # Table of contents inclusion for documentation
├── .github/workflows/CODEOWNERS      # GitHub CODEOWNERS for repository management
├── .github/workflows/deploy-preview.yml # GitHub Actions workflow for deploying preview builds
├── .github/workflows/release-docs.yml  # GitHub Actions workflow for releasing documentation
├── Gemfile                            # Ruby Gem dependencies for Jekyll and plugins
├── index.md                           # Home page of the documentation site
├── readme.md                          # Main documentation file
```

## Directory & File Descriptions
- `_config_preview.yml`: Jekyll configuration for the preview deployment, specifying the base URL for the preview environment.
- `_config_release.yml`: Configuration for the release version deployment, defining the base URL for versioned documentation.
- `_config_theme.yml`: Specifies settings for the "Just the Docs" theme used in the documentation site.
- `_config.yml`: Core configuration file for the entire Jekyll site, including themes, plugins, and collections.
- `_docs/`: Directory containing various documentation files written in Markdown.
  - `dockerhub.md`: Markdown file outlining the integration with Docker Hub for building and pushing images using GitHub Actions.
  - `gitlab-registry.md`: Contains details about implementing Blue/Green deployment strategies using GitLab Container Registry alongside Shepherd.
  - `index.md`: A summary and navigation point for the documentation project.
  - `my-docs.md`: Main documentation file providing instructions for the project.
  - `new.md`: Example content for a new child documentation page.
  - `n8n.md`: Guides for deploying n8n on Kubernetes with TLS and basic authentication.
  - `new-task.md`: Documentation for practical tasks associated with Docker Swarm.
  - `nginx-blue-green.md`: Explains how to implement a Blue-Green deployment strategy with NGINX and Docker Swarm.
  - `shepherd.md`: Configuration for Shepherd related to DockerHub authentication.
  - `workshop-diagrams.md`: Contains ASCII diagrams to visualize workflows and deployments.
- `_includes/toc.md`: Template inclusion file for generating a table of contents.
- `.github/workflows/`: Contains workflow files for GitHub Actions.
  - `CODEOWNERS`: Defines team members responsible for reviewing various parts of the repository.
  - `deploy-preview.yml`: Workflow for deploying preview documentation upon pushes to the develop branch.
  - `release-docs.yml`: Workflow for the manual release of documentation.
- `Gemfile`: Specifies the Ruby dependencies necessary for the functionality and plugins of the Jekyll project.
- `index.md`: Entry point for users visiting the documentation site.
- `readme.md`: Provides general instructions for using the repository.

## Deployment & Setup
1. **Clone the Repository**: 
   ```
   git clone https://github.com/<username>/my-docs.git
   cd my-docs
   ```
2. **Install Ruby Dependencies**:
   Make sure you have Ruby and Bundler installed. Then run:
   ```
   bundle install
   ```
3. **Serve the Site Locally**:
   To preview your documentation site locally, run:
   ```
   bundle exec jekyll serve
   ```

## Configuration & Environment
Environment variables and configurations are managed through the `.github/workflows/` YAML files for CI/CD processes and within the various `_config.yml` files to control site behavior.

### Important Environment Variables:
- `GITHUB_TOKEN`: Required for GitHub Actions to authenticate and deploy content.

## Troubleshooting
- If you encounter issues while deploying, check GitHub Action logs for errors regarding permissions or dependency installation.
- Ensure your token has the necessary scopes for writing content to the repository.

## Scripts & Automation
The GitHub Actions workflows automate the deployment process:
- **`deploy-preview.yml`**: Automatically builds and deploys the documentation site to the `gh-pages` branch on pushes to `develop`.
- **`release-docs.yml`**: Manages the creation of versioned releases of the documentation manually triggered through GitHub.

This setup encapsulates development best practices, ensuring seamless integration and documentation maintenance.