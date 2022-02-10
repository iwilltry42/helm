# Collabora CODE

Collabora is an online office suite with a free [Collabora Online Development Edition (CODE)](https://www.collaboraoffice.com/code/).

## Introduction

This chart creates a scalable Collabora CODE Deployment to run Collabora CODE e.g. for usage with Nextcloud.  
The setup is based on the [official CODE docker documentation](https://sdk.collaboraonline.com/docs/installation/CODE_Docker_image.html).
The default setup terminates SSL at the ingress controller (nginx, traefik, whatever) with certificates automatically generated e.g. by cert-manager.

## Installing the Chart

To install the chart with the release name `collabora-code`, run:

```bash
helm repo add iwilltry42 https://iwilltry42.github.io/helm/
helm install --name collabora-code iwilltry42/collabora
```

## Uninstalling the Chart

To uninstall the `collabora-code` release:

```bash
helm uninstall collabora-code
```

## Configuration

Collabora is configured using environment variables as per the official [Collabora Docker configuration](https://sdk.collaboraonline.com/docs/installation/CODE_Docker_image.html#how-to-configure-docker-image).

All parameters in `/etc/loolwsd/loolwsd.xml` can be adjusted in the `extra_params` environment variable, so you don't have to store a config file anywhere, making Collabora Code stateless.

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`. For example,

```bash
$ helm install --name collabora-code \
    --set varname=true iwilltry42/collabora
```

Alternatively, pass in a values.yaml file:

```bash
helm install --name collabora-code -f values.yaml iwilltry42/collabora
```

| Parameter                                         | Description                                                   | Default                                                     |
| ------------------------------------------------- | ------------------------------------------------------------- | ----------------------------------------------------------- |
| `replicaCount`                                    | Number of provisioner instances to deployed                   | `1`                                                         |
| `strategy`                                        | Specifies the strategy used to replace old Pods by new ones   | `Recreate`                                                  |
| `image.repository`                                | Provisioner image                                             | `collabora/code`                                            |
| `image.tag`                                       | Version of provisioner image                                  | `21.11.1.4.1`                                                   |
| `image.pullPolicy`                                | Image pull policy                                             | `IfNotPresent`                                              |
| `collabora.DONT_GEN_SSL_CERT`                     | Disable generating a new SSL certificate signed by a dummy CA. | `true`                                                      |
| `collabora.domain`                                | Double escaped WOPI host (e.g. Nextcloud)                                      | `wopihost\\.domain\\.tld`                                         |
| `env.extra_params`                          | List of extra parameters to set in `/etc/coolwsd/coolwsd.xml`                              | `--o:ssl.termination=true --o:ssl.enable=false`             |
| `env.server_name`                           | Collabora server name (single escaped)                        | `collabora\.domain\.tld`                                         |
| `env.dictionaries`                          | Collabora enabled dictionaries                                | `de_DE en_GB en_US es_ES fr_FR it nl pt_BR pt_PT ru`        |
| `env.password`                              | Collabora admin panel password                                    | `examplepass`                                               |
| `secret.username`                              | Collabora admin panel username                                    | `admin`                                                     |
| `existingSecret` | Reference an existing secret which will be injected as environment variables, e.g. to not have username/password from the `collabora` section in the values file. | `""` |
| `ingress.enabled`                                 |                                                               | `false`                                                     |
| `ingress.annotations`                             |                                                               | `{}`                                                        |
| `ingress.paths`                                   |                                                               | `[]`                                                        |
| `ingress.hosts`                                   |                                                               | `[]`                                                        |
| `ingress.tls`                                     |                                                               | `[]`                                                        |
| `livenessProbe.enabled`                           | Turn on and off liveness probe                                | `true`                                                      |
| `livenessProbe.initialDelaySeconds`               | Delay before liveness probe is initiated                      | `30`                                                        |
| `livenessProbe.periodSeconds`                     | How often to perform the probe                                | `10`                                                        |
| `livenessProbe.timeoutSeconds`                    | When the probe times out                                      | `2`                                                         |
| `livenessProbe.successThreshold`                  | Minimum consecutive successes for the probe                   | `1`                                                         |
| `livenessProbe.failureThreshold`                  | Minimum consecutive failures for the probe                    | `3`                                                         |
| `livenessProbe.scheme`                            | Scheme for the probe                                          | `HTTP`                                                      |
| `livenessProbe.path`                              | Path for the probe                                            | `/`                                                         |
| `readinessProbe.enabled`                          | Turn on and off readiness probe                               | `true`                                                      |
| `readinessProbe.initialDelaySeconds`              | Delay before readiness probe is initiated                     | `30`                                                        |
| `readinessProbe.periodSeconds`                    | How often to perform the probe                                | `10`                                                        |
| `readinessProbe.timeoutSeconds`                   | When the probe times out                                      | `2`                                                         |
| `readinessProbe.successThreshold`                 | Minimum consecutive successes for the probe                   | `1`                                                         |
| `readinessProbe.failureThreshold`                 | Minimum consecutive failures for the probe                    | `3`                                                         |
| `readinessProbe.scheme`                           | Scheme for the probe                                          | `HTTP`                                                      |
| `readinessProbe.path`                             | Path for the probe                                            | `/`                                                         |
| `securityContext.allowPrivilegeEscalation`        | Create & use Pod Security Policy resources                    | `true`                                                      |
| `securitycontext.capabilities.add`                | Collabora needs to run with MKNOD as capabibility             | `[MKNOD]`                                                   |
| `resources`                                       | Resources required (e.g. CPU, memory)                         | `{}`                                                        |
| `nodeSelector`                                    | Node labels for pod assignment                                | `{}`                                                        |
| `affinity`                                        | Affinity settings                                             | `{}`                                                        |
| `tolerations`                                     | List of node taints to tolerate                               | `[]`                                                        |
