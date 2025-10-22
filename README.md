```
# My Docs Automation – Documentation and CI/CD Pipeline Guide

This repository is designed for automation of documentation generation and deployment using Jekyll, GitHub Actions, and Docker. It provides a structured approach to manage project documentation in a way that can be easily updated and deployed to GitHub Pages with CI/CD practices. The primary technologies used include Jekyll for website generation, Ruby for dependency management, and Docker for environment handling.

## File Tree
```
.
├── _config_preview.yml              # Configuration for the preview deployment
├── _config_release.yml              # Configuration for release builds
├── _config_theme.yml                # Theme configuration for Jekyll
├── _config.yml                      # Main configuration file for Jekyll
├── _docs/dockerhub.md               # Documentation for Docker Hub automation
├── _docs/gitlab-registry.md         # Documentation for Blue/Green deployment using GitLab
├── _docs/index.md                   # Main index for the documentation site
├── _docs/my-docs.md                 # Documentation task overview
├── _docs/my-docs/new.md             # Child documentation page
├── _docs/n8n.md                     # Guide for deploying n8n in Kubernetes
├── _docs/new-task.md                # Practical task on containerization and management
├── _docs/nginx-blue-green.md         # Documentation for Blue-Green deployment strategy
├── _docs/shepherd.md                 # Guide for Shepherd docker automation
├── _docs/workshop-diagrams.md        # Workshop diagrams for visual understanding
├── _includes/toc.md                 # Table of contents for documentation pages
├── .github/workflows/CODEOWNERS     # Specifies code owners for the repository
├── .github/workflows/deploy-preview.yml  # GitHub Actions workflow for deploying preview
├── .github/workflows/release-docs.yml    # GitHub Actions workflow for manual releases
├── Gemfile                           # Ruby gem dependencies for Jekyll
├── index.md                          # Home page of the documentation site
├── readme.md                         # This README file
```

## Architecture & Structure
The repository is organized primarily into configurations, documentation sources, and CI/CD workflows. The `_docs` folder contains the markdown documentation related to various tasks, while `_config.yml` and associated files define how the Jekyll site should be built. The workflows in the `.github/workflows` directory automate the deployment processes.

1. **Configuration Files:** 
   - `_config.yml`, `_config_theme.yml`, `_config_preview.yml`, `_config_release.yml` manage Jekyll's settings and themes.
   
2. **Documentation:** 
   - All documents within `_docs/`. Each markdown file serves educational purposes for different tasks or technologies.

3. **Automation Scripts:**
   - The workflows in `.github/workflows` handle the CI/CD operations automatically on code changes or manual triggers for documentation updates.

## Deployment & Setup
To set up and deploy the project locally or on GitHub:

1. **Clone the Repository:**
   ```bash
   git clone https://github.com/username/my-docs.git
   cd my-docs
   ```

2. **Install Ruby and Bundler:**
   Ensure you have Ruby installed. If not, install it using RVM or a package manager. Install Bundler:
   ```bash
   gem install bundler
   ```

3. **Install Dependencies:**
   Run the following in the root directory:
   ```bash
   bundle install
   ```

4. **Build the Site:**
   Use the appropriate config for the desired build (like preview or release):
   ```bash
   bundle exec jekyll build --config _config.yml,_config_theme.yml,_config_preview.yml -d _site
   ```

5. **Serve Locally:**
   You can also serve it locally to preview:
   ```bash
   bundle exec jekyll serve --config _config.yml,_config_theme.yml,_config_preview.yml
   ```

## Configuration & Environment
### Environment Variables
- Ensure the following environment variables are configured for workflows:
  - `GITHUB_TOKEN`: Required for GitHub actions to authenticate and publish content.

### Dependencies
- Ruby (version should match the one in `Gemfile`)
- Bundler
- Jekyll (specified in `Gemfile`)

## Usage
After building the site, you can access your documentation locally using the preview link provided by the Jekyll server. For deployment:
- Push to the `develop` branch to trigger the `Deploy Preview` workflow which generates a live preview.
- Manually trigger the `Manual Doc Release` workflow for production releases.

## Troubleshooting
- **Website Not Building:** Ensure all Ruby gems are installed correctly. Re-run `bundle install` to check for any missing gems.
- **CI/CD Fails on GitHub:** Check that the environment variables are set correctly in GitHub Actions settings under the repository secrets.

## Automation & CI/CD
This project employs GitHub Actions for CI/CD:
- **Deploy Preview Workflow** (`deploy-preview.yml`): Triggered on pushes to `develop` branch, builds and deploys to the `gh-pages` branch under the `preview` directory.
- **Release Workflow** (`release-docs.yml`): Manually executed to deploy specific versions of documentation.

Following this structure and guidelines, you can easily manage your project documentation and deployment processes efficiently. 
```
