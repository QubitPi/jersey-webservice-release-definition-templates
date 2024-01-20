Jersey Webservice Template JPA Release Definition
=================================================

[![Screwdriver CD badge][Screwdriver CD badge]][Screwdriver CD url]
[![HashiCorp Packer badge][HashiCorp Packer badge]][HashiCorp Packer url]
[![HashiCorp Terraform badge][HashiCorp Terraform badge]][HashiCorp Terraform url]
[![Apache License badge]][Apache License url]
[![GitHub Workflow Status][GitHub Workflow Status badge]][GitHub Workflow Status url]

A set of [Screwdriver CD template]s that deploys [immutable][Immutable Infrastructure] instances of instantiated [Jersey Webservice Template]s to AWS. It uses the
[screwdriver-template-main npm package] to assist with template validation, publishing, and tagging. 

This release definition contains the following templates, _each corresponding to a branch in
[Jersey Webservice Template] GitHub repo_:

- [Deploying the `master` branch without SSL/HTTPS or any other addons](./sd-template-basic.yaml)
- [Deploying the `master` branch with SSL/HTTPS support](./sd-template-ssl.yaml)
- [Deploying the `jpa-elide` branch without SSL/HTTPS or any other addons](./sd-template-jpa.yaml)

All templates tag the latest versions with the `latest` tag.

How to Use Templates
--------------------

There are 3 steps that needs to be
[overridden](https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates#overriding-template-steps): 

1. **clone-webservice** - the 'git clone' command that pulls webservice repo and _the command must clone it to `../ws`
2. **load-properties-file** - The commands that loads
   [webservice properties files](https://qubitpi.github.io/jersey-webservice-template/docs/elide/configuration) and they
   must load them into `../ws/src/main/resources/` directory
3. **clone-data-models** - the 'git clone' command that pulls
   [data models](https://qubitpi.github.io/jersey-webservice-template/docs/elide/data-model) repo and _the command must 
   clone it to `../data-models`

```yaml
---
jobs:
  main:
    requires: [~pr, ~commit]
    template: QubitPi/jersey_webservice_template_jpa_release_definition@latest
    steps:
      - clone-webservice: git clone git@github.com:my-org/my-ws.git ../ws
      - load-properties-file: |
          echo "$OAUTH_PROPERTIES" > ../ws/src/main/resources/oauth.properties
          echo "$APPLICATION_PROPERTIES" > ../ws/src/main/resources/application.properties
          echo "$JPADATASTORE_PROPERTIES" > ../ws/src/main/resources/jpadatastore.properties
      - clone-data-models: git clone https://$DATA_MODEL_PAT_TOKEN@github.com/my-org/my-data-models.git ../data-models

    secrets:
      - AWS_WS_PKRVARS_HCL
      - SSL_CERTIFICATE
      - SSL_CERTIFICATE_KEY
      - NGINX_CONFIG_FILE
      - FILEBEAT_CONFIG_FILE
      - AWS_WS_TFVARS
      - MAVEN_SETTINGS_XML
      - OAUTH_PROPERTIES
      - APPLICATION_PROPERTIES
      - JPADATASTORE_PROPERTIES
      - DATA_MODEL_PAT_TOKEN
      - AWS_ACCESS_KEY_ID
      - AWS_SECRET_ACCESS_KEY
```

The following [Screwdriver Secrets] needs to be defined before running this template:

> [!TIP]
> Please refer to
> [Jersey Webservice Template documentation](https://qubitpi.github.io/jersey-webservice-template/docs/configuration#cicd)
> on what each of those secrets are and how to obtain them.

- AWS_WS_PKRVARS_HCL
- SSL_CERTIFICATE
- SSL_CERTIFICATE_KEY
- NGINX_CONFIG_FILE
- AWS_WS_TFVARS
- MAVEN_SETTINGS_XML
- AWS_ACCESS_KEY_ID
- AWS_SECRET_ACCESS_KEY

> [!NOTE]
> See [Screwdriver's Template documentation][Screwdriver CD template] for more information.

License
-------

The use and distribution terms for [Jersey Webservice Template JPA Release Definition] are covered by the
[Apache License, Version 2.0].

<div align="center">
    <a href="https://opensource.org/licenses">
        <img align="center" width="50%" alt="License Illustration" src="https://github.com/QubitPi/QubitPi/blob/master/img/apache-2.png?raw=true">
    </a>
</div>

[Apache License badge]: https://img.shields.io/badge/Apache%202.0-F25910.svg?style=for-the-badge&logo=Apache&logoColor=white
[Apache License url]: https://www.apache.org/licenses/LICENSE-2.0
[Apache License, Version 2.0]: http://www.apache.org/licenses/LICENSE-2.0.html

[GitHub Workflow Status badge]: https://img.shields.io/github/actions/workflow/status/QubitPi/jersey-webservice-release-definition-templates/ci-cd.yml?branch=master&logo=github&style=for-the-badge
[GitHub Workflow Status url]: https://github.com/QubitPi/jersey-webservice-release-definition-templates/actions/workflows/ci-cd.yml

[HashiCorp Packer badge]: https://img.shields.io/badge/Packer-02A8EF?style=for-the-badge&logo=Packer&logoColor=white
[HashiCorp Packer url]: https://qubitpi.github.io/hashicorp-packer/packer/docs
[HashiCorp Terraform badge]: https://img.shields.io/badge/Terraform-7B42BC?style=for-the-badge&logo=terraform&logoColor=white
[HashiCorp Terraform url]: https://qubitpi.github.io/hashicorp-terraform/terraform/docs

[Jersey Webservice Template JPA]: https://qubitpi.github.io/jersey-webservice-template/docs/category/jpa-through-yahooelide
[Jersey Webservice Template JPA Release Definition]: https://github.com/QubitPi/jersey-webservice-template-jpa-release-definition

[Screwdriver CD template]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates
[Screwdriver Secrets]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/configuration/secrets
[screwdriver-template-main npm package]: https://github.com/QubitPi/screwdriver-cd-template-main
[Screwdriver CD url]: https://qubitpi.github.io/screwdriver-cd-homepage/
[Screwdriver CD badge]: https://img.shields.io/badge/Screwdriver%20CD-1475BB?style=for-the-badge&logo=data:image/svg+xml;base64,PD94bWwgdmVyc2lvbj0iMS4wIiBlbmNvZGluZz0iaXNvLTg4NTktMSI/Pg0KPCEtLSBVcGxvYWRlZCB0bzogU1ZHIFJlcG8sIHd3dy5zdmdyZXBvLmNvbSwgR2VuZXJhdG9yOiBTVkcgUmVwbyBNaXhlciBUb29scyAtLT4NCjxzdmcgaGVpZ2h0PSI4MDBweCIgd2lkdGg9IjgwMHB4IiB2ZXJzaW9uPSIxLjEiIGlkPSJMYXllcl8xIiB4bWxucz0iaHR0cDovL3d3dy53My5vcmcvMjAwMC9zdmciIHhtbG5zOnhsaW5rPSJodHRwOi8vd3d3LnczLm9yZy8xOTk5L3hsaW5rIiANCgkgdmlld0JveD0iMCAwIDUxMiA1MTIiIHhtbDpzcGFjZT0icHJlc2VydmUiPg0KPHBhdGggc3R5bGU9ImZpbGw6I2ZmZmZmZjsiIGQ9Ik01MDQuNzgzLDc3LjA5MWgtMC4wMDZIMzAzLjQ5NGMtMi4wMzYsMC0zLjk3NywwLjg1OS01LjM0NSwyLjM2Ng0KCWMtMS4zNjgsMS41MDgtMi4wMzgsMy41Mi0xLjg0Miw1LjU0OWwzLjkyNSw0MC42OTNjMC4zNDMsMy41NTIsMy4yMjgsNi4zMiw2Ljc5Miw2LjUxNmw0Mi4wNSwyLjMxNGwtODAuNzM2LDExMi4wMjNMMTgwLjI5Myw3MS4xNDINCglsNjMuNTYzLTIuNDNjMy44NDQtMC4xNDYsNi44OTYtMy4yNzYsNi45NDUtNy4xMjFsMC40NjQtMzYuMzkyYzAuMDIyLTEuOTMyLTAuNzI2LTMuNzkyLTIuMDgyLTUuMTY2DQoJYy0xLjM1Ni0xLjM3Mi0zLjIwNi0yLjE0Ny01LjEzNy0yLjE0N0g3LjIyYy0zLjk4OSwwLTcuMjIsMy4yMzEtNy4yMiw3LjIyMXY0MC41NThjMCwzLjk0NywzLjE3LDcuMTYzLDcuMTE1LDcuMjJsNjYuOTE3LDAuOTY0DQoJbDEyNy4yNTcsMjI0LjQ4OGwtMC41NjgsMTQwLjIwNWwtODguNDgsMy42MzFjLTMuODE3LDAuMTU1LTYuODUsMy4yNTktNi45MjMsNy4wNzdsLTAuNzE2LDM3LjUwNg0KCWMtMC4wMzcsMS45MzksMC43MDksMy44MSwyLjA2OSw1LjE5NmMxLjM1NiwxLjM4MywzLjIxMiwyLjE2MSw1LjE1MSwyLjE2MWgyNzYuNzYyYzEuOTgxLDAsMy44NzUtMC44MTMsNS4yMzktMi4yNDkNCgljMS4zNjMtMS40MzUsMi4wNzgtMy4zNjgsMS45NzQtNS4zNDZsLTEuOTMzLTM3LjEzMmMtMC4xOTItMy42OTItMy4xNC02LjY0MS02LjgzNS02LjgzNGwtNzYuMTI0LTMuOTkybC0xMS4wODItMTM2LjU3DQoJTDQzNi40NzksMTM3LjMzbDU1LjY0LTIuNTNjMy4wNjMtMC4xNDIsNS43MDQtMi4yMDIsNi41ODctNS4xMzlsMTIuOTA5LTQzLjAxOGMwLjI0OC0wLjcyOSwwLjM4NS0xLjUxNiwwLjM4NS0yLjMzMg0KCUM1MTIsODAuMzI1LDUwOC43NzIsNzcuMDkxLDUwNC43ODMsNzcuMDkxeiIvPg0KPC9zdmc+
