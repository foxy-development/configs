# Shared Configs/Workflows

- [GitHub Actions](#github-actions)
  - [Deploy (`deploy.yaml`)](#deploy-deployyaml)
  - [Maven Build + Dockerize (`maven-docker.yaml`)](#maven-build--dockerize-maven-dockeryaml)

## GitHub Actions

### Deploy (`deploy.yaml`)

Connects to Tailscale and deploys the latest version of the specified `service-name` using
Watchtower.

| Input          | Type    | Description                       | Required | Default |
| -------------- | ------- | --------------------------------- | -------- | ------- |
| `service-name` | boolean | The name of the service to deploy | Yes      | N/A     |

### Maven Build + Dockerize (`maven-docker.yaml`)

Builds a Maven project and (on mainline) creates a Docker image, optionally deploying it.

| Input          | Type    | Description                                                           | Required | Default                    |
| -------------- | ------- | --------------------------------------------------------------------- | -------- | -------------------------- |
| `image-name`   | string  | The image name to publish the package as                              | No       | `${{ github.repository }}` |
| `deploy`       | boolean | If the docker image should be pushed and deployed for the main branch | No       | True                       |
| `service-name` | string  | The name of the service to deploy                                     | No       | `${{ github.repository }}` |
