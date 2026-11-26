# Workflow Analysis

## 1. What triggers this workflow to run?

This workflow runs when code is pushed to the `main` branch and when a pull request is opened or updated for the `main` branch.

The trigger is defined in the `on` section:

```yaml
on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]
```

## 2. What are the four main steps this workflow performs?

The Build and Test job performs four main steps:

1. Checkout code
2. Validate HTML
3. Check links
4. Upload artifact

After those steps are completed successfully, the Deploy job performs:

5. Deploy to GitHub Pages

## 3. What does the "Checkout code" step do and why is it necessary?

The "Checkout code" step downloads the repository files into the GitHub Actions runner. This step is necessary because the workflow must have access to the project's files before it can validate HTML, check links, or deploy the website.

## 4. What is the purpose of the environment configuration?

The environment configuration specifies that the website will be deployed to the `github-pages` environment. It also stores the URL of the deployed website so GitHub can track and display deployment information.

```yaml
environment:
  name: github-pages
  url: ${{ steps.deployment.outputs.page_url }}
```

## 5. How does this automated deployment improve reliability compared to manual deployment?

Automated deployment improves reliability because every deployment follows the same process. The workflow automatically validates the website, checks for issues, uploads the files, and deploys the site. This reduces human error, ensures consistency, and makes deployments faster and more dependable.

## 6. What would happen if you pushed code to a different branch (not main)?

The workflow would still run the Build and Test job if a pull request was created targeting the `main` branch. However, the website would not be deployed because the Deploy job is configured to run only when code is pushed directly to the `main` branch.

The deployment condition is:

```yaml
if: github.event_name == 'push' && github.ref == 'refs/heads/main'
```

Therefore, pushes to branches other than `main` will not trigger a GitHub Pages deployment.
