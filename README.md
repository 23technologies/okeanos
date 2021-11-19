# okeanos

## Hetzner Cloud

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
