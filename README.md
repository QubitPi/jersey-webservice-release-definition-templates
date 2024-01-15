Jersey Webservice Template JPA Release Definition
=================================================

[![Apache License Badge]](https://www.apache.org/licenses/LICENSE-2.0)

A [Screwdriver CD template] that deploys [Jersey Webservice Template JPA]. It uses the
[screwdriver-template-main npm package] to assist with template validation, publishing, and tagging.

This release definition contains the following template:

1. [Deploying the JPA webservice to AWS using HashiCorp](./sd-template.yaml)

It tags the latest versions with the `latest` tag.

How to Use This Template
------------------------

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

[Apache License Badge]: https://img.shields.io/badge/Apache%202.0-F25910.svg?style=for-the-badge&logo=Apache&logoColor=white
[Apache License, Version 2.0]: http://www.apache.org/licenses/LICENSE-2.0.html

[Jersey Webservice Template JPA]: https://qubitpi.github.io/jersey-webservice-template/docs/category/jpa-through-yahooelide
[Jersey Webservice Template JPA Release Definition]: https://github.com/QubitPi/jersey-webservice-template-jpa-release-definition

[Screwdriver CD template]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/templates
[Screwdriver Secrets]: https://qubitpi.github.io/screwdriver-cd-guide/user-guide/configuration/secrets
[screwdriver-template-main npm package]: https://github.com/QubitPi/screwdriver-cd-template-main
