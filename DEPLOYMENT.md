# N8N Deployment Guide

## Overview

This document describes how to trigger deployments for the n8n application.

## Automatic Deployment Trigger

The n8n deployment is automatically triggered when changes are made to the `shah.mdc` file.

### How it Works

1. **Trigger File**: The `shah.mdc` file serves as a deployment trigger
2. **Workflow**: When `shah.mdc` is modified and pushed to `master` or `copilot/**` branches, the `deploy-on-shah-trigger.yml` workflow is activated
3. **Docker Build**: The workflow then triggers the `docker-build-push.yml` workflow, which:
   - Builds Docker images for both AMD64 and ARM64 platforms
   - Pushes the images to the Docker registry
   - Performs security scanning on the images

### Triggering a Deployment

To trigger a deployment:

1. Modify the `shah.mdc` file (e.g., add a timestamp or deployment note)
2. Commit and push the changes to the repository
3. The deployment workflow will automatically start

Example:
```bash
echo "Deployment triggered at: $(date -u +"%Y-%m-%dT%H:%M:%SZ")" >> shah.mdc
git add shah.mdc
git commit -m "Trigger deployment"
git push
```

## Manual Deployment Trigger

You can also manually trigger the deployment workflow through the GitHub Actions interface:

1. Go to the repository's "Actions" tab
2. Select the "Docker: Build and Push" workflow
3. Click "Run workflow"
4. Configure the deployment parameters as needed
5. Click "Run workflow" to start the deployment

## Deployment Types

The deployment workflow supports different release types:

- **stable**: Production releases with versioned tags
- **nightly**: Automated nightly builds (runs daily at midnight)
- **branch**: Branch-specific builds for testing
- **dev**: Development builds for pull requests

## Monitoring Deployments

You can monitor deployment progress in the GitHub Actions tab:

- Navigate to the repository's "Actions" tab
- Look for the "Deploy on Shah Trigger" or "Docker: Build and Push" workflows
- Click on a workflow run to see detailed logs

## Security

All Docker images are automatically scanned for security vulnerabilities using Trivy before being pushed to the registry.
