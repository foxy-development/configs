# Shared Configs/Workflows

- [GitHub Actions](#github-actions)
  - [Deploy (`deploy.yaml`)](#deploy-deployyaml)
  - [Maven Build + Dockerize (`maven-docker.yaml`)](#maven-build--dockerize-maven-dockeryaml)
  - [API Documentation Generator (`api-docs.yaml`)](#api-documentation-generator-api-docsyaml)

## GitHub Actions

### Deploy (`deploy.yaml`)

Connects to Tailscale and deploys the latest version of the specified `service-name` using
Watchtower.

| Input          | Type    | Description                       | Required | Default |
| -------------- | ------- | --------------------------------- | -------- | ------- |
| `service-name` | boolean | The name of the service to deploy | Yes      | N/A     |

| Secret                | Required | Description                   |
| --------------------- | -------- | ----------------------------- |
| `TS_OAUTH_CLIENT_ID`  | Yes      | Tailscale Oauth client ID     |
| `TS_OAUTH_SECRET`     | Yes      | Tailscale Oauth client secret |
| `DEPLOY_SSH_USERNAME` | Yes      | Deployment server username    |
| `DEPLOY_SSH_HOST`     | Yes      | Deployment server hostname    |

### Maven Build + Dockerize (`maven-docker.yaml`)

Builds a Maven project and (on mainline) creates a Docker image, optionally deploying it.

| Input          | Type    | Description                                                           | Required | Default                    |
| -------------- | ------- | --------------------------------------------------------------------- | -------- | -------------------------- |
| `image-name`   | string  | The image name to publish the package as                              | No       | `${{ github.repository }}` |
| `deploy`       | boolean | If the docker image should be pushed and deployed for the main branch | No       | True                       |
| `service-name` | string  | The name of the service to deploy                                     | No       | `${{ github.repository }}` |

| Secret                | Required                   | Description                   |
| --------------------- | -------------------------- | ----------------------------- |
| `TS_OAUTH_CLIENT_ID`  | If `inputs.deploy == True` | Tailscale Oauth client ID     |
| `TS_OAUTH_SECRET`     | If `inputs.deploy == True` | Tailscale Oauth client secret |
| `DEPLOY_SSH_USERNAME` | If `inputs.deploy == True` | Deployment server username    |
| `DEPLOY_SSH_HOST`     | If `inputs.deploy == True` | Deployment server hostname    |

### API Documentation Generator (`api-docs.yaml`)

Generates API documentation which is pushed to https://github.com/foxy-development/api-docs.

| Input             | Type   | Description                                                      | Required | Default                              |
| ----------------- | ------ | ---------------------------------------------------------------- | -------- | ------------------------------------ |
| `definition-path` | string | The (globbable) path to OpenAPI definition files                 | No       | `src/main/resources/api/**/api.yaml` |
| `name-expression` | string | A `bash` expression to extract the service name from the `$path` | No       | `$(basename ${path%/api.yaml})`      |

| Secret         | Required | Description                                                    |
| -------------- | -------- | -------------------------------------------------------------- |
| `API_DOCS_PAT` | Yes      | A Personal Access Token with access to the api-docs repository |
