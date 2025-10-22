```
# Documentation for My Docs Site

## Overview
This repository is designed for managing and automating documentation processes using Jekyll. The primary purpose is to facilitate the handling of documentation projects by leveraging the capabilities of GitHub Actions for deployment and automation. The documentation includes guides on CI/CD tasks, Docker management, and blue-green deployment strategies along with essential configurations.

## File Tree
```
.
├── _config_preview.yml               # Preview configuration for Jekyll
├── _config_release.yml               # Release configuration for Jekyll
├── _config_theme.yml                 # Theme configuration for Jekyll
├── _config.yml                       # Main configuration for Jekyll
├── _docs/dockerhub.md                # Documentation for Docker Hub Cloud Build with GitHub Actions
├── _docs/gitlab-registry.md          # Documentation for Blue/Green Deploy with GitLab Container Registry and Shepherd
├── _docs/index.md                    # Index file for the documentation
├── _docs/my-docs.md                  # Main documentation content file
├── _docs/my-docs/new.md              # New child page in the documentation structure
├── _docs/n8n.md                      # Guide for deploying n8n in Kubernetes
├── _docs/new-task.md                 # Practical task on containerization and management assignment
├── _docs/nginx-blue-green.md          # Documentation on Blue-Green strategy with Shepherd and NGINX
├── _docs/shepherd.md                 # Documentation on Shepherd DockerHub authentication with Swarm setup
├── _docs/workshop-diagrams.md         # ASCII diagrams for practical workshops
├── _includes/toc.md                  # Table of contents for documentation pages
├── .github/workflows/CODEOWNERS      # GitHub CODEOWNERS file for repository management
├── .github/workflows/deploy-preview.yml # Workflow for deploying preview builds
├── .github/workflows/release-docs.yml # Workflow for manual documentation release
├── Gemfile                           # Ruby dependencies management file
├── index.md                          # Home page for the documentation
├── readme.md                         # General project readme
```

## Deployment & Setup
To set up and deploy this documentation project, follow these steps:

1. **Clone the Repository**
   ```bash
   git clone <repository-url>
   cd <repository-name>
   ```

2. **Install Dependencies**
   Make sure you have Ruby and Bundler installed. Then run:
   ```bash
   bundle install
   ```

3. **Run Jekyll Locally**
   Start a local server to preview the documentation:
   ```bash
   bundle exec jekyll serve
   ```
   Navigate to `http://localhost:4000` in your browser to view the documentation site.

4. **GitHub Actions Workflows**
   - The `deploy-preview.yml` workflow deploys the preview version to GitHub Pages when changes are pushed to the `develop` branch.
   - The `release-docs.yml` workflow allows for manual releases of the documentation by specifying a version tag.

## Configuration & Environment
- Jekyll configuration is mainly handled through `.yml` files:
  - `_config.yml`: Main configuration file for Jekyll site
  - `_config_preview.yml`: Configuration for building preview sites
  - `_config_release.yml`: Configuration for building release versions
- Ensure all necessary environment variables like `GITHUB_TOKEN` are set in GitHub secrets for deployments.

## Troubleshooting
- **Common Issues**
  - If Jekyll fails to build, ensure all dependencies in the `Gemfile` are satisfied.
  - For GitHub Actions failures, check the workflow logs for specific error messages related to the deployment process.

- **Debugging Steps**
  - Review the logs generated during the GitHub Action runs.
  - Test the configuration files locally with `jekyll build` to check for configuration errors before pushing changes.

## Scripts & Automation
- The GitHub Actions workflows (`deploy-preview.yml` and `release-docs.yml`) automate the deployment process of documentation to GitHub Pages.
- Set up your project repository according to the structure outlined and use the workflows to manage how documentation updates are published and versioned efficiently.

By following this documentation, you should be able to contribute effectively to the project and understand how to maintain and deploy the documentation efficiently.