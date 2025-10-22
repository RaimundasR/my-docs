```
# Documentation for My Docs Automation Project

## Overview
This repository hosts a documentation automation project utilizing Jekyll to facilitate the creation, versioning, and deployment of markdown-based documentation. The project employs GitHub Actions for CI/CD processes, automating the preview and release of documentation through Docker Hub and GitLab Container Registry integrations.

## File Tree
```
.
├── _config_preview.yml              # Configuration for preview builds
├── _config_release.yml              # Configuration for release builds
├── _config_theme.yml                # Configuration for theming
├── _config.yml                      # Main configuration for Jekyll site
├── _docs/dockerhub.md               # Documentation for Docker Hub integration
├── _docs/gitlab-registry.md         # Documentation for GitLab Container Registry setup
├── _docs/index.md                   # Index page for documentation
├── _docs/my-docs.md                 # Additional documentation handling
├── _docs/my-docs/new.md             # New child page under my docs
├── _docs/n8n.md                     # Deployment guide for n8n using Kubernetes
├── _docs/new-task.md                # Guide for managing Docker Swarm
├── _docs/nginx-blue-green.md        # Blue-green deployment strategy documentation
├── _docs/shepherd.md                 # Documentation for Shepherd integration
├── _docs/workshop-diagrams.md       # Diagrams for workshop use cases
├── _includes/toc.md                 # Table of contents for documentation pages
├── .github/workflows/CODEOWNERS     # GitHub CODEOWNERS file for repository oversight
├── .github/workflows/deploy-preview.yml # GitHub Actions workflow for deploying preview versions
├── .github/workflows/release-docs.yml # GitHub Actions workflow for manual doc releases
├── Gemfile                           # Dependency specification for Jekyll
├── index.md                          # Main landing index file for the site
├── readme.md                         # Project readme file
```

## Deployment & Setup

### Prerequisites:
- Ruby (version 3.1 or above)
- Bundler (version 2.4.22 or above)
- Jekyll (version 4.3.2 or above)
- Docker (if using Docker for deployment)

### Installation Steps:
1. Clone the repository:
   ```bash
   git clone https://<repository-url>.git
   cd <repository-folder>
   ```

2. Install Ruby dependencies:
   ```bash
   bundle install
   ```

3. Build the site:
   ```bash
   bundle exec jekyll build --config _config.yml,_config_theme.yml,_config_preview.yml -d _site
   ```

4. For Docker deployment, execute:
   ```bash
   # Your Docker specific deployment commands here
   ```

## Configuration & Environment
### Required Configuration Files:
- `_config.yml`: Main configuration for site settings.
- `_config_preview.yml`: Configuration specifically for preview documentation builds.
- `_config_release.yml`: Configuration used for release builds.

### Environment Variables:
- Ensure that `GITHUB_TOKEN` is set in your GitHub repository secrets for automated actions.
- Additional secrets for Docker authentication may be required in CI workflows.

## Troubleshooting
### Common Issues:
- **GitHub Actions Failures**:
  - Check permissions in the `.github/workflows/*` files to ensure they match your GitHub repository settings.
  - Verify your secrets are correctly set for authenticated pulls and deployments.

- **Dependency Issues**:
  - Ensure all Ruby gems are properly installed with `bundle install`.

### Logging and Debugging:
For more detailed logs during the CI/CD processes, consider enabling the debug mode in your action workflows by exporting `DEBUG` to `true`.

## Scripts & Automation
This repository includes CI/CD automation through GitHub Actions:
- **Deploy Preview**: Automatically deploys the preview build of the documentation to a specified branch.
- **Release Docs**: Facilitates the manual creation of documentation releases, archiving them for version control.

Each workflow can be found in the `.github/workflows/` directory and can be manually triggered or scheduled based on your CI/CD strategy.

--- 

Utilize this documentation as a guide for understanding the structure and operational flow of the My Docs Automation Project.  Ensure all environment configurations are set correctly before running any tasks or actions. Happy documenting!