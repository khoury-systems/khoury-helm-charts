# Hugo Docs System

This chart is used to deploy a [Hugo](https://gohugo.io) static site generator container, along with a [git-sync](https://github.com/kubernetes/git-sync) container for staying up to date with a repo. It an also support an [OAuth](https://github.com/OAuth2-proxy/OAuth2-proxy) proxy as well. It can be used to target any hugo site repo, and run multiple instances for different branches. It assumes some prerequisites are already present.

## Prerequisites 

1. Kubernetes cluster to install into.
2. Git repo with hugo site code.
3. A deploy key in that repo with at least Read access.
4. A secret on the cluster with the private deploy key, as well as a known-hosts list for the git server's SSH fingerprint. (`git_sync.git.secretName` specified in the [values](./values.yaml) file must have 2 keys-value pairs, `ssh` with the private key and `known_hosts` with the git server's fingerprint.)
5. An ingress controller and DNS set for any ingress you want to serve.
6. (If SSL required) A premade SSL certificate to reference, cert-manager to generate one, or a reverse proxy.

If OAuth is also required, the following are necessary:
1. An OIDC compatible system.
2. A client configured on the OIDC system.
3. A configuration file in the TOML format as per [docs](https://oauth2-proxy.github.io/oauth2-proxy/configuration/overview#config-options). (An example compatible with Keycloak is provided in the [values](./values.yaml) file.)
 
## Get Repository Info
```console
helm repo add khoury https://helm.khoury.northeastern.edu/
helm repo update
```

_See [helm repo](https://helm.sh/docs/helm/helm_repo/) for command documentation._

## Install Chart

```console
helm install [RELEASE_NAME] khoury/hugo-docs
```

_See [configuration](#configuring) below._

_See [helm install](https://helm.sh/docs/helm/helm_install/) for command documentation._

```console
helm uninstall [RELEASE_NAME]
```

This removes all the Kubernetes components associated with the chart and deletes the release.

_See [helm uninstall](https://helm.sh/docs/helm/helm_uninstall/) for command documentation._

## Upgrading Chart

```console
helm upgrade [RELEASE_NAME] khoury/hugo-docs --install
```

_See [helm upgrade](https://helm.sh/docs/helm/helm_upgrade/) for command documentation._

## Configuring

See [Customizing the Chart Before Installing](https://helm.sh/docs/intro/using_helm/#customizing-the-chart-before-installing). To see all configurable options with detailed comments, visit the chart's [values.yaml](./values.yaml), or run these configuration commands:

```console
helm show values khoury/hugo-docs
```