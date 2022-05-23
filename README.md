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
