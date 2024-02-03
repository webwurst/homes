# OpenID Connect Provider

https://dexidp.io/


[Register a new OAuth application](https://github.com/settings/applications/new)

- Application name: dex.webwur.st
  Homepage URL: https://dex.webwur.st
  Application description: OpenID Connect Provider
  Authorization callback URL: https://dex.webwur.st/callback

- `Client ID` 
    -> ./helm.yaml//values.config.connectors[id=github].config.clientID
  `Client secret` 
    -> ./github-connector-secret

- Add org and teams to connectors config.
    -> ./helm.yaml//values.config.connectors[id=github].config.orgs


For each app choose and create new:

- `Client ID` 
    -> ./helm.yaml//values.config.staticClients[id=<id>].id
  `Client secret` 
    For example `$ openssl rand -base64 30`
    -> ./helm.yaml//values.config.staticClients[id=<id>].secretEnv 
      -> ./kustomization.yaml//secretGenerator[name=oidc-secrets] 
        -> `$ sops --enrypt "./<id>-client-secret"`

- Also place those in the apps config.
