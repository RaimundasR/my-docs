# CI/CD Documentation Automation – Setup and Usage Guide

## Overview / Purpose
This repository provides a comprehensive solution for automating the generation, deployment, and management of documentation using Jekyll and GitHub Actions. It streamlines a CI/CD pipeline for documentation, ensuring that all changes in the codebase are automatically reflected in the documentation hosted on GitHub Pages. Key technologies utilized include Jekyll for static site generation, Ruby for dependency fulfillment, and GitHub Actions for continuous integration and deployment automation.

## File Tree
```
.
├── _docs
│   ├── my-docs
│   │   └── new.md  # new documentation child page
│   ├── dockerhub.md  # guide for Docker Hub integration
│   ├── gitlab-registry.md  # guidance for GitLab Container Registry
│   ├── index.md  # main index page for documentation
│   ├── my-docs.md  # detailed documentation on CI/CD tasks
│   ├── n8n.md  # deployment guide for n8n in Kubernetes
│   ├── new-task.md  # practical task related to Docker Swarm
│   ├── nginx-blue-green.md  # blue-green deployment strategy
│   ├── shepherd.md  # Shepherd automation integration
│   └── workshop-diagrams.md  # diagrams for practical tasks
├── _includes
│   └── toc.md  # table of contents for documentation
├── .github
│   └── workflows
│       ├── CODEOWNERS  # code owner definitions for reviews
│       ├── deploy-preview.yml  # workflow for deploying preview changes
│       └── release-docs.yml  # manual release workflow for documentation
├── _config_preview.yml  # configuration for preview builds
├── _config_release.yml  # configuration for release builds
├── _config_theme.yml  # theme configuration for Jekyll
├── _config.yml  # main configuration for Jekyll site
├── Gemfile  # Ruby Gem dependencies for the project
├── index.md  # home/index page of the documentation
└── README.md  # project overview and documentation guide
```

## Directory & File Descriptions
- `_docs`: Contains all the markdown files for documentation.
- `_includes`: Stores reusable code snippets and components such as the table of contents.
- `.github/workflows`: Holds GitHub Actions workflows for automating CI/CD processes.
- `_config_preview.yml`: Jekyll configuration settings specific to preview deployments.
- `_config_release.yml`: Jekyll configuration settings specific to release deployments.
- `_config_theme.yml`: Theme settings for the Jekyll site.
- `_config.yml`: The main Jekyll site configuration file.
- `Gemfile`: Lists the Ruby gems required for the project dependencies.
- `index.md`: The main landing page for the documentation site.

## Setup / Installation / Deployment
To set up and deploy this documentation site, follow these steps:

1. **Clone the repository**:
   ```
   git clone https://github.com/your_username/repository_name.git
   cd repository_name
   ```

2. **Install Ruby and Bundler**:
   Ensure Ruby is installed. Use the following command to install Bundler:
   ```
   gem install bundler
   ```

3. **Install dependencies**:
   Run the following command in the root directory of the project:
   ```
   bundle install
   ```

4. **Build the documentation site**:
   To build the site locally, execute:
   ```
   bundle exec jekyll serve
   ```
   This will start a local server at `http://localhost:4000`.

5. **Deploy the site**:
   Automated deployment occurs via GitHub Actions. Push changes to the `develop` branch for preview deployment, or manually trigger the release workflow via the GitHub Actions configuration.

## Usage
Once the setup is complete, you can create or edit markdown files in the `_docs` directory. After committing and pushing those changes, GitHub Actions will handle deployments, making the updated documentation accessible to users.

To view your documentation locally, navigate to `http://localhost:4000` after starting the Jekyll server.

## Configuration / Environment
- **Environment Variables**: Ensure all necessary variables (like `GITHUB_TOKEN` for deployments) are set in your GitHub repository settings.
- **Dependencies**:
  - Ruby (version 3.1)
  - Bundler (version 2.4.22)
  - Jekyll (version 4.3.2)
  - Additional Jekyll plugins specified in the Gemfile.

## Troubleshooting
- If the website does not compile or display correctly, verify for errors in markdown files.
- Ensure that GitHub secrets are properly configured for deployments.
- Confirm that the correct Ruby version is being used and all dependencies are installed by executing `bundle install`.

## Automation / CI/CD / Scripts
This repository includes several GitHub Actions workflows:
- **Deploy Preview Workflow** (`deploy-preview.yml`): Automatically builds and deploys documentation to a preview URL when changes are pushed to the `develop` branch.
- **Manual Release Workflow** (`release-docs.yml`): Supports manual triggering of documentation releases, specifying the version for deployment, which includes building and deploying the documentation to GitHub Pages.

By adhering to these guidelines, you can effectively manage your documentation project using a structured CI/CD workflow, ensuring rapid updates and seamless deployment processes.

```
```
