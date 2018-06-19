# AWX

AWX provides a web-based user interface, REST API, and task engine built on top of Ansible.


## TL;DR

```console
$ helm repo add incubator http://storage.googleapis.com/kubernetes-charts-incubator
$ helm install -n awx incubator/awx
```


## Introduction

This chart bootstraps a [AWX](https://github.com/ansible/awx) Deployment on a [Kubernetes](https://kubernetes.io) cluster
using the [Helm](https://helm.sh) package manager. It provisions a fully featured AWX installation.
For more information on AWX and its capabilities, see its [documentation](https://docs.ansible.com/ansible-tower/index.html) and [Docker Hub repository](https://hub.docker.com/u/ansible/).


## Installing the Chart

To install the chart with the release name `awx`:

```console
$ helm install -n awx incubator/awx
```


## Uninstalling the Chart

To uninstall/delete the `awx` deployment:

```console
$ helm delete awx
```


## Configuration

The following table lists the configurable parameters of the AWX chart and their default values.

Parameter | Description | Default
--- | --- | ---
`dbImage.repository` | DB image repository | `postgres`
`dbImage.tag` | DB image version | `9.6.2`
`dbUser` | DB username | `awx`
`dbPassword` | DB user password | `awxpass`
`dbDatabaseName` | DB name | `awx`
`dbService.port` | DB service port | `5432`
`persistence.accessMode` | DB PV access mode. If you select file storage(NAS), you must set "ReadWriteMany". If you select block storage(ISCSI), you must set "ReadWriteOnce". It is based on IBM Cloud. | `ReadWriteOnce`
`persistence.size` | DB PV size. You can set at least 20GB. It is based on IBM Cloud. | `20Gi`
`persistence.claimName` | DB claim name | `awx-db`
`persistence.annotations` | DB PV Storage-Class selection. Storage class is defined as follows: "ibmc-(block or file)-(bronze or silver or gold)". | `ibmc-block-bronze`
`persistence.mountPath` | DB PV mount path | `/var/lib/postgresql/data`
`persistence.subPath` | DB PV sub-path (with DB file) | `/pgdata`
`persistence.name` | DB persistence volume name | `data`
`persistence.billingType` | DB PV hourly payment method. It is charged in the IBM Cloud. | `hourly`
`queueImage.repository` | Queue image repository | `rabbitmq`
`queueImage.tag` | Queue image version | `3`
`queueUser` | Queue username | `guest`
`queuePassword` | Queue user password | `guest`
`queueVhost` | Queue application vhost | `awx`
`queueService.port` | Queue service port | `5672`
`cacheImage.repository` | Cache image repository | `memcached`
`cacheImage.tag` | Cache image version | `alpine`
`cacheService.port` | Cache service port | `11211`
`taskImage.repository` | AWX-task image repository | `ansible/awx_task`
`taskImage.tag` | AWX-task image version | `latest`
`secretKey` | AWX secret key | `aabbcc`
`taskCommand` | Command to be executed at awx-task runtime. | `["/bin/sh", "-c", "echo 'root::0:0:root:/root:/bin/bash' >> /etc/passwd;su -c 'pip install softlayer pywinrm';su -c 'yum install -y wget'"]`
`webImage.repository` | Web image repository | `ansible/awx_web`
`webImage.tag` | Web image version | `latest`
`webService.type` | Set it to "NodePort". To change to "LoadBalancer" and "Ingress", you can set it after "Portable IP" is preassigned. It is based on IBM Cloud. | `NodePort`
`webService.internalPort` | Set internal port | `80`
`webService.externalPort` | Set external port | `8052`
`ingress.enabled` | If true, an ingress is be created | `false`
`ingress.annotations` | Annotations for the ingress | `{}`
`ingress.path` | Path for backend | `/`
`ingress.hosts` | A list of hosts for the ingresss | `[]`
`ingress.tls.secretName` | If tls is enabled, uses an existing secret with this name | `""`
`ingress.tls.hosts` | A list of hosts for   | `""`


Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart. For example,

```bash
$ helm install --name awx -f values.yaml awx
```