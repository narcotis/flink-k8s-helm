# Apache Flink Helm Chart

This is an implementation of https://ci.apache.org/projects/flink/flink-docs-stable/ops/deployment/kubernetes.html

This chart will install session cluster https://ci.apache.org/projects/flink/flink-docs-stable/ops/deployment/kubernetes.html#flink-session-cluster-on-kubernetes.
If you are interested in supporting session/job clusters: https://github.com/GoogleCloudPlatform/flink-on-k8s-operator

## Pre Requisites:

* Kubernetes 1.9 with alpha APIs enabled and support for storage classes

* PV support on underlying infrastructure

## StatefulSet Details

* https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/

## StatefulSet Caveats

* https://kubernetes.io/docs/concepts/workloads/controllers/statefulset/#limitations

## Chart Details

This chart will do the following:

* Implement a dynamically scalable Flink(Jobmanagers and Taskmanagers) cluster using Kubernetes StatefulSets

### Installing the Chart

To install the chart with the release name `flink` in the default
namespace:

```
$ helm repo add flink-k8s https://narcotis.github.io/flink-k8s-helm
$ helm repo update
$ helm install --name flink flink-k8s/flink
```

If using a dedicated namespace(recommended) then make sure the namespace
exists with:

```
$ helm repo add flink-k8s https://narcotis.github.io/flink-k8s-helm
$ helm repo update
$ helm install --name flink --namespace flink flink-k8s/flink
```

| Parameter                                | Description                                                                                                                                                              | Default                |
|------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------|
| `image.repository`                       | Flink Container image name                                                                                                                                               | `flink`                |
| `image.tag`                              | Flink Container image tag                                                                                                                                                | `1.17.0-scala_2.12-java8`    |
| `image.PullPolicy`                       | Flink Containers pull policy                                                                                                                                             | `IfNotPresent`         |
| `flink.monitoring.enabled`               | Enables Flink monitoring                                                                                                                                                 | `false`                 |
| `jobmanager.highAvailability.enabled`    | Enables Jobmanager HA mode key                                                                                                                                           | `true`                |
| `jobmanager.highAvailability.storageDir` | storageDir for Jobmanager in HA mode                                                                                                                                     | `null`                 |
| `jobmanager.replicaCount`                | Jobmanagers count context                                                                                                                                                | `1`                    |
| `jobmanager.heapSize`                    | Jobmanager HeapSize options                                                                                                                                              | `1g`                   |
| `jobmanager.resources`                   | Jobmanager resources                                                                                                                                                     | `{}`                   |
| `taskmanager.resources`                  | Taskmanager Resources key                                                                                                                                                | `{}`                   |
| `taskmanager.heapSize`                   | Taskmanager heapSize mode                                                                                                                                                | `1g`                   |
| `jobmanager.replicaCount`                | Taskmanager count context                                                                                                                                                | `1`                    |
| `taskmanager.numberOfTaskSlots`          | Number of Taskmanager taskSlots resources                                                                                                                                | `1`                    |
| `taskmanager.resources`                  | Taskmanager resources                                                                                                                                                    | `{}`                   |
| `secrets.bitnamiSealedSecrets.enabled`   | Enables creation of [Sealed Secrets](https://github.com/bitnami-labs/sealed-secrets)                                                                                     | `false`                |

### Install with HA

You can install this chart with enabled HA based on Kubernetes by provided follow parameters:
```
$ helm install --name flink flink-k8s/flink --set \
zookeeper.enabled=true,jobmanager.replicaCount=2,jobmanager.highAvailability.enabled=true,jobmanager.highAvailability.storageDir=s3://MY_BUCKET/flink/jobmanager
```
* storageDir can be different for your installation, see 
  https://ci.apache.org/projects/flink/flink-docs-stable/ops/config.html#high-availability-storagedir
