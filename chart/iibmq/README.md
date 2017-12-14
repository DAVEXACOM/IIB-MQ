

# IBM MQ

![MQ Logo](https://developer.ibm.com/messaging/wp-content/uploads/sites/18/2017/07/IBM-MQ-Square-200.png)

IBM® MQ is messaging middleware that simplifies and accelerates the integration of diverse applications and business data across multiple platforms. It uses message queues to facilitate the exchanges of information and offers a single messaging solution for cloud, mobile, Internet of Things (IoT) and on-premises environments.

# IBM INTEGRATION BUS

![IIB Logo](https://ot4i.github.com/iib-helm/ibm-integration-bus-dev/IBM_Integration_Bus_Icon.svg)

IBM® Integration Bus is a market-leading lightweight enterprise integration engine that offers a fast, simple way for systems and applications to communicate with each other. As a result, it can help you achieve business value, reduce IT complexity and save money. IBM Integration Bus supports a range of integration choices, skills and interfaces to optimize the value of existing technology investments. 


# Introduction

This chart deploys a single IBM MQ Advanced for Developers server (queue manager) and an IBM integration bus runtime into an IBM Cloud private or other Kubernetes environment.

## Prerequisites

- Kubernetes 1.7 or greater, with beta APIs enabled
- If persistence is enabled (see [configuration](#configuration)), then you either need to create a PersistentVolume, or specify a Storage Class if classes are defined in your cluster.

## Installing the Chart

To install the chart with the release name `foo`:

```sh
helm install --name foo iib-mq --set license=accept
```

This command accepts the [IBM MQ Advanced for Developers license](LICENSE) and the [IBM Integration Bus for Developers license](LICENSE) and deploys an IBM Integration Bus for Developers server and an MQ Advanced for Developers server on the Kubernetes cluster. The [configuration](#configuration) section lists the parameters that can be configured during installation.

> **Tip**: See all the resources deployed by the chart using `kubectl get all -l release=foo`

## Uninstalling the Chart

To uninstall/delete the `foo` release:

```sh
helm delete foo
```

The command removes all the Kubernetes components associated with the chart, except any Persistent Volume Claims (PVCs).  This is the default behavior of Kubernetes, and ensures that valuable data is not deleted.  In order to delete the Queue Manager's data, you can delete the PVC using the following command:

```sh
kubectl delete pvc -l release=foo
```

## Configuration
The following table lists the configurable parameters of the `iib-mq` chart and their default values.

| Parameter                        | Description                                     | Default                                                    |
| -------------------------------- | ----------------------------------------------- | ---------------------------------------------------------- |
| `license`                        | Set to `accept` to accept the terms of the IBM license  | `not accepted`                                     |
| `image.repository`               | Image full name including repository            | `ibmcom/mq`                                                |
| `image.tag`                      | Image tag                                       | `9`                                                        |
| `image.pullPolicy`               | Image pull policy                               | `IfNotPresent`                                             |
| `image.pullSecret`               | Image pull secret, if you are using a private Docker registry | `nil`                                        |
| `persistence.enabled`           | Use persistent volumes for all defined volumes                  | `true`                                     |
| `persistence.useDynamicProvisioning` | Use dynamic provisioning (storage classes) for all volumes | `true`                                     |
| `dataPVC.name`                  | Suffix for the PVC name                                         | `"data"`                                   |
| `dataPVC.storageClassName`      | Storage class of volume for main MQ data (under `/var/mqm`)     | `""`                                       |
| `dataPVC.size`                  | Size of volume for main MQ data (under `/var/mqm`)              | `2Gi`                                      |
| `service.name`                   | Name of the Kubernetes service to create        | `qmgr`                                                     |
| `service.type`                   | Kubernetes service type exposing ports, e.g. `NodePort`       | `ClusterIP`                                  |
| `resources.limits.cpu`          | Kubernetes CPU limit for the Queue Manager container | `500m`                                                   |
| `resources.limits.memory`       | Kubernetes memory limit for the Queue Manager container | `512Mi`                                              |
| `resources.requests.cpu`        | Kubernetes CPU request for the Queue Manager container | `500m`                                                 |
| `resources.requests.memory`     | Kubernetes memory request for the Queue Manager container | `512Mi`                                            |
| `queueManager.name`              | MQ Queue Manager name                           | Helm release name                                          |
| `queueManager.dev.adminPassword` | Developer defaults - administrator password     | Random generated string.  See the notes that appear when you install for how to retrieve this.                            |
| `queueManager.dev.appPassword`   | Developer defaults - app password   | `nil` (no password required to connect an MQ client)                   |
| `nameOverride`                   | Set to partially override the resource names used in this chart | `nil`                                      |
|`nodename`              | IBM Integration Bus integration node name                           | `IIBV10010`                                        |
| `servername`              | IBM Integration Bus integration server name                           | `IS1`                                        |

Specify each parameter using the `--set key=value[,key=value]` argument to `helm install`.

Alternatively, a YAML file that specifies the values for the parameters can be provided while installing the chart.

> **Tip**: You can use the default [values.yaml](values.yaml)

## Persistence

The chart mounts a [Persistent Volume](http://kubernetes.io/docs/user-guide/persistent-volumes/).

# Copyright

© Copyright IBM Corporation 2017

