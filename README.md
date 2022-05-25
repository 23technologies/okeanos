# User documentation for Okeanos

[Okeanos](https://okeanos.dev/) is a fully-managed service based on Gardener
to deliver Kubernetes clusters at scale everywhere, currently in public beta.

[Login to the dashboard](https://dashboard.okeanos.dev) is possible with your
existing GitHub account. After the first login a new project will be
automatically created for you. Please reload the dashboard after a few seconds
to see this project. You can then add your credentials for the cloud to be used
and create Kubernetes clusters that will use virtual machines in that cloud
account, and we provide and manage the Kubernetes Control Plane on our own
infrastructure.

The clouds currently available are AWS, GCP, Azure, Hetzner Cloud, Fugacloud,
Betacloud and CityCloud. We are currently working on support for IONOS and OVH.
During the public beta, the only cost for using Okeanos is the cost on your
own cloud account, charged directly by the cloud provider.

We do not ask for payment information and will not charge you for the control
plane of the managed Kubernetes clusters. The service and support is offered
on a best effort basis for free accounts until further notice.

## Hetzner Cloud

The following describes how a [Hetzner Cloud](https://www.hetzner.com/de/cloud)
project is integrated into Okeanos.

* Login to the [Hetzner Cloud Console](https://console.hetzner.cloud/projects), click the
  ``NEUES PROJEKT`` button and create a new project, e.g. with the name ``okeanos-rocks``.

![Create new project](/images/hcloud/create-new-project.png)

* Now switch to the dashboard of the new project and select the
  ``API TOKENS`` tab in the ``Sicherheit`` panel.

![API tokens list](/images/hcloud/security-api-tokens.png)

* There click on ``API-TOKEN HINZUFÜGEN`` and create a new API token with the permission
  ``Lesen & Schreiben`` and as description e.g. ``okeanos-api-token``.

![Create new API token](/images/hcloud/create-new-api-token.png)

* Copy the created token.

![Generated API token](/images/hcloud/generated-api-token.png)

* Next, go to the [Okeanos login page](https:/dashboard.okeanos.dev).

![Okeanos dashboard login](/images/dashboard-login.png)

* Log in with your GitHub account.

![Login with GitHub](/images/dashboard-login-dex.png)

* Switch to the ``SECRETS`` panel and click the ``+`` icon in the
  ``Infrastructure Secrets`` section.

![Secrets panel](/images/dashboard-secrets.png)

* Paste the previously copied Hetzner Cloud API token and create the secret with
  the name e.g. ``okeanos-rocks-hcloud-secret``.

![Add new Hetzner Cloud Secret](/images/dashboard-secrets-new-hcloud.png)

* Now a new cluster can be created on the Hetzner Cloud.

![Create new cluster](/images/dashboard-clusters-create.png)

## Grafana

The provided Grafana is stateless and therefore has the flaw of not
beeing able to save settings. For example marking a dashboard as a
favorite is impossible.

* Once you've opened a dashboard and return to the dashboards overview,
you'll see in *Recently viewed dashboards*. Try to add it to favorites.

![Dashboard overview](/images/grafana/grafana_error_try_to_add_favorite.png)

* You'll get an error pop-up stating you are *Unauthorized*

![Error](/images/grafana/grafana_error_unauthorized_notification.png)

* Now try to open any dashboard (or any other function actually)

![Try opening a dashboard again](/images/grafana/grafana_error_click_on_dashboard_again.png)

* Grafana requires you to login again.

![Login required](/images/grafana/grafana_error_login_required.png)

## Programmatic shoot creation

If you want to create clusters trough applications, you can do this via
a custom resource definition of type *shoot* trough kubernetes.
To gain access to the gardener API which creates shoots, you have
to create a service account from the web dashboard.

![Create new Service Account](/images/dashboard-members-create-service-account.png)

From here you can also view or download the kubernetes config file.
Once your *kubectl* or other tool is configured to use the new config
file, you can simple apply the resource, e.g.

`kubectl apply -f cluster.yaml`

**cluster.yaml** example for [Betacloud](https://www.betacloud.de/)

```yaml
kind: Shoot
apiVersion: core.gardener.cloud/v1beta1
metadata:
  name: my-cluster-name
  namespace: garden-<your_project_name>
spec:
  cloudProfileName: betacloud
  hibernation:
    enabled: false
    schedules:
      - start: '00 17 * * 1,2,3,4,5'
        end: '00 08 * * 1,2,3,4,5'
        location: Europe/Berlin
  kubernetes:
    version: 1.22.9
  networking:
    type: cilium
    pods: 100.73.0.0/16
    nodes: 10.250.0.0/16
    services: 100.88.0.0/13
  provider:
    type: openstack
    controlPlaneConfig:
      apiVersion: openstack.provider.extensions.gardener.cloud/v1alpha1
      kind: ControlPlaneConfig
      loadBalancerProvider: amphora
    infrastructureConfig:
      apiVersion: openstack.provider.extensions.gardener.cloud/v1alpha1
      floatingPoolName: external
      kind: InfrastructureConfig
      networks:
        workers: 10.250.0.0/16
    workers:
      - cri:
          name: containerd
        name: worker-small
        machine:
          type: 2C-4GB-40GB
          image:
            name: gardenlinux
            version: 576.1.0
        maximum: 2
        minimum: 1
        maxSurge: 1
        maxUnavailable: 0
        volume:
          size: 50Gi
  purpose: development
  region: betacloud-1
  secretBindingName: betacloud-secret
```

Keep in mind, that some things need to be changed accordingly.
Here are at least a few explanations of the bare minimum possible.

* **metadata.name**: The name you desire for the new cluster, must be unique in project
* **metadata.namespace**: your project name with a *garden-* prefix
* **spec.cloudProfileName**: Name of the cloud you want your cluster to be created on
* **spec.networking.type**: *calico* or *cilium* currently
* **spec.provider.type**: Type of cloud (AWS, Openstack, etc.)
* **spec.provider.workers[0].machine.type**: Flavor of your worker nodes
* **spec.purpose**: Purpose of the new kubernetes cluster
* **spec.secretBindingName**: The secret to use when connecting to the cloud
