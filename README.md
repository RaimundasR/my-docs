```
# Repository Overview

This repository hosts a collection of automated documentation projects using **Jekyll**. It exemplifies the CI/CD pipeline for documentation deployment via GitHub Actions and Docker configurations. It includes different operational guides, tutorial documents, and templates to facilitate the automation and deployment processes in a DevOps environment.

## File Tree

```
.
├── _config_preview.yml              # Preview configuration for Jekyll documentation site
├── _config_release.yml              # Release configuration for Jekyll documentation site
├── _config_theme.yml                # Theme configuration for Jekyll documentation site
├── _config.yml                      # Main configuration file for Jekyll documentation site
├── _docs/dockerhub.md               # Documentation for Docker Hub CI/CD workflows
├── _docs/gitlab-registry.md         # Documentation for blue/green deployment using GitLab Registry
├── _docs/index.md                   # Main index for the collection of documentation
├── _docs/my-docs.md                 # Detailed guide for basic CI/CD task documentation
├── _docs/my-docs/new.md             # Child page for "my-docs" documentation section
├── _docs/n8n.md                     # Guide for deploying n8n on Kubernetes
├── _docs/new-task.md                # Practical task for containerization and management
├── _docs/nginx-blue-green.md        # Documentation on Blue-Green deployment strategies
├── _docs/shepherd.md                 # Guide on using Shepherd with Docker Swarm
├── _docs/workshop-diagrams.md       # ASCII diagrams for workshop tasks
├── _includes/toc.md                 # Table of contents template for Jekyll documentation
├── .github/workflows/CODEOWNERS     # File to define code owner for specific files/directories
├── .github/workflows/deploy-preview.yml  # GitHub Actions workflow for deploying previews
├── .github/workflows/release-docs.yml    # GitHub Actions workflow for manual document release
├── Gemfile                           # Ruby gem dependencies for Jekyll
├── index.md                          # Homepage index for the documentation site
├── readme.md                         # General instructions or notes for the repository
```

## Setup / Installation / Deployment Steps

To set up this documentation platform, follow these steps:

1. **Clone the Repository**
   ```bash
   git clone https://github.com/your_username/my-docs.git
   cd my-docs
   ```

2. **Install Required Dependencies**
   Make sure you have Ruby and Bundler installed. Then, install the project dependencies.
   ```bash
   bundle install
   ```

3. **Build the Site**
   Build your site by running:
   ```bash
   bundle exec jekyll build
   ```

4. **Previewing Changes**
   To preview the changes, you can run:
   ```bash
   bundle exec jekyll serve
   ```

5. **Deploying Changes**
   Changes can be deployed using provided GitHub Actions workflows:
   - **Deploy Preview**: Activate the preview workflow for branches as follows:
   ```bash
   git push origin develop
   ```
   - **Release Documentation**: Trigger a manual release from the GitHub Actions interface by specifying a version for the documentation.

## Services and Environment Variables

This repository utilizes GitHub Actions for CI/CD workflows. The following environment variables are used:
- `GITHUB_TOKEN`: This token is automatically created by GitHub Actions, allowing access to repository settings.
  
Additionally, ensure to configure GitHub secrets for:
- `DOCKER_USER`: Your Docker Hub username.
- `DOCKER_PAT`: Docker Hub Personal Access Token.

## Troubleshooting

Common issues and quick resolutions:
1. **Jekyll Build Errors**
   - Ensure all dependencies are correctly installed. Check for any version mismatches in your `Gemfile`.

2. **GitHub Actions Failures**
   - Review the logs in Actions tab on GitHub to identify issues with CI/CD workflows.
  
3. **Deployment Issues**
   - Verify that the respective workflows (deploy-preview and release-docs) are correctly configured. Ensure that the proper triggering events are set.

## File Descriptions

- **_config.yml**: Main configuration file for the Jekyll documentation project containing site title and layout configurations.
- **_config_preview.yml**: Configuration for building the documentation site with a preview setting.
- **_config_release.yml**: Configuration for release-specific settings when deploying the documentation.
- **_docs/**: Directory containing all markdown files for documentation purposes including operational guides, service setups, and tutorials.

By installing and deploying this repository, you can automate the creation and maintenance of your documentation using the powerful capabilities of Jekyll and GitHub Actions.