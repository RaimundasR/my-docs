# CI/CD Automation for Documentation – Setup and Usage Guide

## Overview / Purpose
This repository aims to provide a robust solution for automating the build, deployment, and management of documentation using Jekyll and GitHub Actions. It facilitates a seamless CI/CD process for documentation, ensuring that any changes made to the codebase are automatically reflected in the published documentation hosted on GitHub Pages. The repository leverages technologies such as Jekyll for static site generation, Ruby for dependency management, and GitHub Actions for workflow automation.

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
└── index.md  # home/index page of the documentation
```

## Directory & File Descriptions
- `_docs`: Contains all the documentation markdown files.
- `_includes`: Holds reusable code snippets and components like the table of contents.
- `.github/workflows`: GitHub Actions workflows for CI/CD automation.
- `_config_preview.yml`: Configuration specific for building the preview versions.
- `_config_release.yml`: Configuration specific for building the release versions.
- `_config_theme.yml`: Configuration settings for the Jekyll theme.
- `_config.yml`: The main Jekyll site configuration.
- `Gemfile`: Specifies the Ruby gems required for the project.
- `index.md`: The main landing page for the documentation site.

## Setup / Installation / Deployment
To set up and deploy this documentation site, follow these steps:

1. **Clone the repository**:
   ```
   git clone https://github.com/your_username/repository_name.git
   cd repository_name
   ```

2. **Install Ruby and Bundler**:
   Ensure you have Ruby installed. You can install Bundler using:
   ```
   gem install bundler
   ```

3. **Install dependencies**:
   Run the following command in the project root:
   ```
   bundle install
   ```

4. **Build the documentation site**:
   To build the site locally, run:
   ```
   bundle exec jekyll serve
   ```
   This will start a local server at `http://localhost:4000`.

5. **Deploy the site**:
   The deployment is automated using GitHub Actions. Push changes to the `develop` branch to trigger a preview deployment or manually trigger a release via the `Manual Doc Release` workflow outlined in the GitHub Actions configuration.

## Usage
Once the setup is complete, you can create or edit markdown files in the `_docs` directory. Upon committing the changes, the GitHub Actions will handle the deployment, making your documentation available to users.

To view your documentation locally, visit `http://localhost:4000` after running the Jekyll server.

## Configuration / Environment
- **Environment Variables**: Ensure that any necessary environment variables (like `GITHUB_TOKEN` for deployment) are set in your GitHub repository settings.
- **Dependencies**: 
  - Ruby (version 3.1)
  - Bundler (version 2.4.22)
  - Jekyll (version 4.3.2)
  - Various other Jekyll plugins specified in the Gemfile.

## Troubleshooting
- If the website does not compile or display correctly, check for errors in your markdown files.
- Ensure that the GitHub Environment secrets are properly configured for deployments.
- Check whether the proper Ruby version is being used and that all dependencies are installed correctly by running `bundle install`.

## Automation / CI/CD / Scripts
The repository includes several GitHub Actions workflows:
- **Deploy Preview Workflow** (`deploy-preview.yml`): Automatically builds and deploys the documentation site to a preview URL whenever changes are pushed to the `develop` branch.
- **Manual Release Workflow** (`release-docs.yml`): Allows manual triggering of the documentation release process, specifying a version for the release, which includes building and deploying the documentation to the GitHub Pages.

By following these guidelines, you can efficiently manage your documentation project using a structured CI/CD workflow, ensuring timely updates and seamless deployment processes for your users.
