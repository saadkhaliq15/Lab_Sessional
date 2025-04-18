# CI/CD Automation with GitHub Actions

This directory contains GitHub Actions workflows that automatically build and deploy Docker images when changes are pushed to the main branch.

## Requirements

To use these workflows, you need to set up the following GitHub secrets:

- `DOCKERHUB_USERNAME`: Your Docker Hub username
- `DOCKERHUB_TOKEN`: A Docker Hub access token with permissions to push images

## How to Create Docker Hub Access Token

1. Log in to [Docker Hub](https://hub.docker.com)
2. Go to Account Settings > Security
3. Create a new access token and copy it
4. Add it as a GitHub repository secret

## Adding GitHub Secrets

1. Go to your GitHub repository
2. Navigate to Settings > Secrets > Actions
3. Click "New repository secret"
4. Add the secrets mentioned above

## Workflow Details

The main workflow:
- Triggers on pushes to the main branch
- Builds Docker images using Dockerfile in the repository 
- Pushes the images to Docker Hub with appropriate tags

## Customizing for Multiple Services

If you have multiple services, you can modify the workflow files to build each service separately:

```yaml
jobs:
  build-service-1:
    runs-on: ubuntu-latest
    steps:
      # Steps for building the first service
      - name: Build and push Service 1
        uses: docker/build-push-action@v4
        with:
          context: ./service-1
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/service-1:latest

  build-service-2:
    runs-on: ubuntu-latest
    steps:
      # Steps for building the second service
      - name: Build and push Service 2
        uses: docker/build-push-action@v4
        with:
          context: ./service-2
          push: ${{ github.event_name != 'pull_request' }}
          tags: ${{ secrets.DOCKERHUB_USERNAME }}/service-2:latest
```
