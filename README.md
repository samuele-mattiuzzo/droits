![CI/CD Pipeline](https://github.com/mcagov/droits/actions/workflows/pipeline-dev.yml/badge.svg)
[![Staging pipeline](https://github.com/mcagov/droits/actions/workflows/pipeline-staging.yml/badge.svg)](https://github.com/mcagov/droits/actions/workflows/pipeline-staging.yml)
[![Production pipeline](https://github.com/mcagov/droits/actions/workflows/pipeline-prod.yml/badge.svg)](https://github.com/mcagov/droits/actions/workflows/pipeline-prod.yml)

# Droits reporting service

The Droits reporting service enables:

- Salvage owners to report their findings with the Maritime & CoastGuard Agency
- Receivers of Wreck to manage these reports

It comprises two applications

1. A public facing frontend etc. in the `webapp/` directory
2. The backoffice API which handles the incoming registrations and is an admin application to handle the reports

## Architecture

![Architecture diagram](./architecture.jpg)

Infrastructure-as-code remains the single source-of-truth for Droits infrastructure! Always check `terraform/` if
unsure.

## Local development

| **dependency**                                                    | **version** |
|-------------------------------------------------------------------|-------------|
| [python](https://www.python.org/)                                 | 3.10.17     |
| [dotnet](https://learn.microsoft.com/en-us/dotnet/)               | 8.0.409     |
| [nvm](https://github.com/nvm-sh/nvm)                              | 0.39.5      |
| [node](https://github.com/nvm-sh/nvm)                             | 18.17.0     |
| [terraform](https://www.terraform.io/)                            | 1.4.6       |
| [Docker desktop](https://www.docker.com/products/docker-desktop/) | Latest      |

To install the dependencies, follow [this guide](/docs/dependencies-setup.md)

### Getting started

To run the application, you'll need the following configuration files:

```bash
webapp/.env.json
backoffice/src/appsettings.json
```

The contents for these files are stored in `1Password`. Please request access from the team if you do not already have it.
Once you have these files in place, you can start the application by running `docker compose up`.

### Troubleshooting

- Instance fails to start: If you ran `docker compose up` before creating and populating the `.env.json` and `appsettings.json` files, this will cause the instance to fail. To resolve this, clean up the environment and run the command again.

- Port 5000 is unavailable: If you encounter a port binding error, port `5000` is already in use. To fix this, update `your docker-compose.yml` file to use an alternative port, such as `5005` or `5010`. **WARNING: do not commit this change**

## Infrastructure-as-code

The [Terraform](./terraform) directory contains the Terraform code for managing the infrastructure for the Droits
reporting service.

## Deployment

Automated testing, building and deployment is performed using GitHub Actions with configuration held in
`.github/workflows`.

### Development environment

A build and deployment to the development environment is triggered on each push to `main`. Docker images are tagged
with the hash of the triggering commit and published to AWS Elastic Container Registry. Images built and deployed to
for the development environment are ephemeral and not used anywhere else.

### Staging environment

A build and deployment to the staging environment is triggered on a manual release set to "pre-release". Docker images are tagged
with the hash of the triggering commit and published to AWS Elastic Container Registry. Images built and deployed to
for the staging environment are ephemeral and not used anywhere else.

### Production environment

A build and deployment to the production environment is triggered on a manual release set to "release". Docker images are tagged
with the hash of the triggering commit and published to AWS Elastic Container Registry. Images built and deployed to
for the production environment are ephemeral and not used anywhere else.

## Secrets

Secrets are set in the Repository and injected during deployment.

## License

Unless stated otherwise, the codebase is released under [the MIT License][mit]. This covers both the codebase and any
sample code in the documentation.

The documentation is [&copy; Crown copyright][copyright] and available under the terms of the [Open Government 3.0][ogl]
licence.

[mit]: LICENCE
[copyright]: http://www.nationalarchives.gov.uk/information-management/re-using-public-sector-information/uk-government-licensing-framework/crown-copyright/
[ogl]: http://www.nationalarchives.gov.uk/doc/open-government-licence/version/3/
