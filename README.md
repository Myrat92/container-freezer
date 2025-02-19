# container-freezer


| STATUS | Sponsoring WG |
| --- | --- |
| Alpha | [Autoscaling](https://github.com/knative/community/blob/main/working-groups/WORKING-GROUPS.md#scaling)|

A standalone service for Knative to pause/unpause containers when request count drops to zero.

## Installation

### Install Knative Serving

In order to use the container-freezer, you will need a running installation of Knative Serving. Instructions for doing so can be found in the [docs](https://knative.dev/docs/admin/install/).

Note that your cluster will need to have the Service Account Token Volume Projection feature enabled. For details, see this link: https://kubernetes.io/docs/tasks/configure-pod-container/configure-service-account/#service-account-token-volume-projection
    
Next, you will need to label your nodes with the runtime used in your cluster:

* For containerd: `kubectl label nodes minikube knative.dev/container-runtime=containerd`

* For docker: `kubectl label nodes minikube knative.dev/container-runtime=docker`
    
### Install the container-freezer daemon

To run the container-freezer service, the first step is to clone this repository:

``` bash
git clone git@github.com:knative-sandbox/container-freezer.git
```

Next, use [`ko`](https://github.com/google/ko) to build and deploy the container-freezer service:

``` bash
cd container-freezer
ko apply -f config/
```

This will install the daemon for the container-freezer, the required permissions, and a service for the daemon. By default, the service has the endpoint http://freeze-service.knative-serving.svc.cluster.local:9696, which we'll need in the next step.

Note: in the future, it will also be possible to install via `kubectl apply -f container-freezer.yaml`

### Enable concurrency endpoint in Knative Serving

By default, Knative does not enable the freezing capability. We can enable it by providing a value for `concurrency-state-endpoint` in the Knative Serving [deployment configmap](https://github.com/knative/serving/blob/main/config/core/configmaps/deployment.yaml):

``` yaml
data:
  concurrency-state-endpoint: "http://$HOST_IP:9696"
```

## Sample application

TBD

