# OpenID Connect Provider

https://dexidp.io/


## Update crds

The dex binary installs crds during startup unless those already exist.
For a more declarative approach copy those from upstream repository:

```bash
curl -L https://github.com/dexidp/dex/archive/refs/tags/v2.38.0.tar.gz \
  | tar -zx --directory ./deploy/dex/crds --strip-components 4 dex-2.38.0/scripts/manifests/crds
```


## Config

[Register a new OAuth application](https://github.com/settings/applications/new)

- Application name: dex.webwur.st
  Homepage URL: https://dex.webwur.st
  Application description: OpenID Connect Provider
  Authorization callback URL: https://dex.webwur.st/callback

- `Client ID` 
    -> ./helm.yaml .values.config.connectors[id=github].config.clientID
  `Client secret` 
    -> ./github-connector-secret

- Add org and teams to connectors config.
    -> ./helm.yaml .values.config.connectors[id=github].config.orgs


For each app choose and create new:

- `Client ID` 
    -> ./helm.yaml .values.config.staticClients[id=<id>].id
  `Client secret` 
    For example `$ openssl rand -base64 30`
    -> ./helm.yaml .values.config.staticClients[id=<id>].secretEnv 
      -> ./kustomization.yaml .secretGenerator[name=oidc-secrets] 
        -> `$ sops --enrypt "./<id>-client-secret"`

- Also place those in the apps config.
