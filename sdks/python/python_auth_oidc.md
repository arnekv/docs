---
pagination_next: null
pagination_prev: null
---

# Authenticate with Azure AD

You can authenticate the Python SDK with Azure AD by **using a token** retrieved when a [user authenticates](#authenticate-with-user-credentials) or with a static [client secret](#authenticate-with-client-secret) for long-running jobs like extractors.

## Prerequisites

- Make sure the CDF project is configured for authentication with Azure AD:

  - [Register the Cognite API and the CDF portal application](https://docs.cognite.com/cdf/access/guides/configure_cdf_azure_oidc) in Azure AD.
  - [Set up Azure AD and CDF groups](https://docs.cognite.com/cdf/access/guides/create_groups_oidc) to control access to CDF data.
  - [Register and configure the relevant applications and components](https://docs.cognite.com/cdf/access/guides/configure_apps_oidc) in Azure AD.

- Install the [Microsoft Authentication Library (MSAL) for Python](https://github.com/AzureAD/microsoft-authentication-library-for-python).

- In all code samples below, you need to specify:

  - `<Tenant ID>` - the ID of the Azure AD tenant where the user is registered.
  - `<Client ID>` - the ID of the application in Azure AD.
  - `<Cluster>` - the cluster where your CDF project is installed. For example, `api` and `westeurope-1`.
  - `<CDF project>` - the name of the CDF project.

    <img className="screenshot" src="/images/cdf/dev/guides/sdk/tenant-and-client-id.png" alt="Client and tenant id" width="750px"/>

If you don't know which values to use for these variables, contact your CDF administrator or [Cognite Support](https://cognite.zendesk.com/hc/en-us/requests/new).

## Authenticate with user credentials

You can authenticate the Python SDK with Azure AD by **using a token** retrieved with user credentials.
To find the examples, see [Credential Providers](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/credential_providers.html#credential-providers).

- You can get the token by letting the user sign in interactively via **a browser** and use the [authenticate with interactive login and token refresh](#authenticate-with-interactive-login-and-token-refresh) flow to access CDF when you're running short-term scripts or using Jupyter.

- If a browser isn't available, for example, if you are logged into a terminal, you can use the [authenticate with user credentials and device code](#authenticate-with-user-credentials-and-device-code) flow.

### Authenticate with interactive login and token refresh

Use this flow to authenticate with user credentials by redirecting to an authentication page in the browser.

You need to provide an `OAuthInteractive` credential provider to the `CogniteClient`. The token is served from an in-memory cache but refreshed if needed (without requiring a user sign in or device code for the refresh).

:::info NOTE
To use this flow with the code sample below, make sure the app is registered in Azure AD as the type **Mobile and desktop applications** with `http://localhost:53000` as the **Redirect URI**.
:::

You can find the code sample [here](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/credential_providers.html#cognite.client.credentials.OAuthInteractive).

### Authenticate with user credentials and device code

If a browser isn't available, for example, if you are logged into a terminal, use this flow to authenticate with user credentials and use a device code to refresh the token.

Here we use the `OAuthDeviceCode` credential provider.

:::info NOTE
To use this flow, select **Allow public client flows** under **Authentication** when registering the app in Azure AD.
:::

You can find the code sample [here](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/cognite_client.html#cognite.client.credentials.OAuthDeviceCode).

## Authenticate with client secret

Use the `OAuthClientCredentials` credential provider for this flow. Use this flow for long-running jobs like extractors.

You can find the code sample [here](https://cognite-sdk-python.readthedocs-hosted.com/en/latest/credential_providers.html#cognite.client.credentials.OAuthClientCredentials).

:::tip Important
The `token_url` should follow the pattern `https://login.microsoftonline.com/$tenantId/oauth2/v2.0/token` for Azure Active Directory, where _$tenantId_ is the ID of the Azure AD tenant where the user is registered.
:::
