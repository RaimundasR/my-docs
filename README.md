# CI/CD Documentation Automation – Deployment and Configuration Guide

## Overview
This repository serves as a centralized documentation automation project utilizing Jekyll for content generation. It primarily focuses on CI/CD workflows to automate deployments and documentation release processes utilizing GitHub Actions. The technologies employed include Ruby, Jekyll, and Docker, enabling a smooth process for maintaining and deploying documentation updates.

## File Tree
```
.
├── _config_preview.yml            # Preview configuration file for Jekyll
├── _config_release.yml            # Configuration file for release builds
├── _config_theme.yml              # Theme configuration for Jekyll
├── _config.yml                    # Main configuration file for Jekyll
├── _docs/dockerhub.md             # Documentation for Docker Hub cloud builds with GitHub Actions
├── _docs/gitlab-registry.md       # Guide for blue/green deployment using GitLab's container registry
├── _docs/index.md                 # Index page for documentation
├── _docs/my-docs.md               # Main documentation file with structure and guides
├── _docs/my-docs/new.md           # A new child page in the 'my-docs' section
├── _docs/n8n.md                   # Deployment guide for n8n in Kubernetes
├── _docs/new-task.md              # Task documentation for containerization and management
├── _docs/nginx-blue-green.md      # Instructions on blue-green deployment strategies using NGINX and Shepherd
├── _docs/shepherd.md               # Shepherd DockerHub authentication and setup guide
├── _docs/workshop-diagrams.md     # ASCII diagrams for workshop tasks
├── _includes/toc.md               # Table of contents for documentation pages
├── .github/workflows/CODEOWNERS   # GitHub CODEOWNERS configuration file
├── .github/workflows/deploy-preview.yml  # GitHub Actions workflow for deploying preview builds
├── .github/workflows/release-docs.yml   # Workflow for manual documentation releases
├── Gemfile                         # Ruby gem dependencies for the project
├── index.md                       # Home page for the documentation site
├── readme.md                      # Basic readme file for the repository
```

## Directory & File Descriptions
- `_config_preview.yml`: Preview configuration file for Jekyll.
- `_config_release.yml`: Configuration file for release builds with specific base url.
- `_config_theme.yml`: Theme configuration for applying styles in Jekyll.
- `_config.yml`: Core configuration file instructing Jekyll on site settings.
- `_docs/dockerhub.md`: Guides users on automating Docker image builds via GitHub Actions.
- `_docs/gitlab-registry.md`: Details deploying applications using GitLab's container registry.
- `_docs/index.md`: Entry point for the documentation site, linking to other resources.
- `_docs/my-docs.md`: Comprehensive documentation with additional guides and topics organized.
- `_docs/my-docs/new.md`: Additional child page to the 'my-docs' documentation section.
- `_docs/n8n.md`: Instructions on deploying n8n with specific configurations in Kubernetes.
- `_docs/new-task.md`: Outlines tasks related to container management and deployment.
- `_docs/nginx-blue-green.md`: Describes blue-green deployment practices leveraging NGINX and Shepherd.
- `_docs/shepherd.md`: Explains setting up Shepherd for DockerHub authentication within a swarm.
- `_docs/workshop-diagrams.md`: Contains ASCII diagrams representing practical workshop tasks.
- `_includes/toc.md`: Includes a template for generating the table of contents on documentation pages.
- `.github/workflows/CODEOWNERS`: Configurations specifying code owners within the repo on GitHub.
- `.github/workflows/deploy-preview.yml`: Sets up a GitHub Action workflow for deploying preview versions of the documentation.
- `.github/workflows/release-docs.yml`: GitHub Action for deploying the live, versioned documentation.
- `Gemfile`: Specifies the Ruby gem dependencies including Jekyll and its plugins.
- `index.md`: The main homepage for navigation and links to other documentation parts.
- `readme.md`: A basic placeholder for the repository README.

## Setup / Installation / Deployment
1. **Clone the Repository**
   ```bash
   git clone https://github.com/your_username/your_repository.git
   cd your_repository
   ```

2. **Install Ruby and Bundler**
   Ensure Ruby is installed on your machine. You can find the instructions on the [official Ruby website](https://www.ruby-lang.org/en/documentation/installation/).
   Install Bundler:
   ```bash
   gem install bundler
   ```

3. **Install Dependencies**
   From the root of the directory, run:
   ```bash
   bundle install
   ```

4. **Build the Documentation Site**
   To build the site using Jekyll with the desired config files:
   ```bash
   bundle exec jekyll build --config _config.yml,_config_theme.yml,_config_preview.yml -d _site
   ```

5. **Serve the Site Locally**
   You can also serve the site on your local environment:
   ```bash
   bundle exec jekyll serve --config _config.yml,_config_theme.yml,_config_preview.yml
   ```

## Usage
- **Accessing Documentation**: After serving the application, access it in your web browser at `http://localhost:4000`.
- **Releasing Documentation**: Utilize the GitHub Actions workflows to automate releases on pushing updates or using manual triggers.

## Configuration / Environment
- Ensure you have the following gem dependencies installed, which are specified in the `Gemfile`:
  - Jekyll
  - Bundler
  - Additional plugins outlined in the `Gemfile`

- **Environment Variables**:
  - Docker environment variables are defined in various documentation files for deployment setups.

## Troubleshooting
- If Jekyll doesn't build correctly:
  - Ensure that all required gems are installed via `bundle install`.
  - Check the syntax and structure of the config files for any discrepancies.

- If documentation is not displaying as expected:
  - Review the `_config.yml` for proper routing and structure settings.

## Automation / CI/CD / Scripts
- **GitHub Actions**: Implemented workflows automate the deployment process:
  - `deploy-preview.yml`: Builds and deploys preview versions upon pushes to the `develop` branch.
  - `release-docs.yml`: Facilitates manual triggering for documentation release, where inputs for versioning can be specified.

- **Shepherd Integration**: The integration with Shepherd for Docker allows for seamless service updates by monitoring image digests.

This repository serves as a comprehensive tool for managing documentation deployment workflows, enhancing productivity and automation in documentation practices.