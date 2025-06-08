---
pagination_next: null
pagination_prev: null
title: Get started
---

# Get started with Seismic SDK

:::info Limited availability
The features described in this section are currently only available to selected customers.
:::

## Installation

To install the seismic SDK package:

```sh
pip install cognite-seismic-sdk
```

## Client setup and authentication

All data in CDF are stored in a CDF tenant identified by a project name. To specify the project or tenant to work on, set the COGNITE_PROJECT environment variable or use the project keyword argument to the CogniteSeismicClient constructor.

The preferred way to authenticate against the Cognite API is by using an OpenID Connect (OIDC) token. Microsoft provides their authetication library (MSAL) that helps generating tokens.

To install the Microsoft Authentication Library (MSAL) package:

```sh
pip install msal
```

To create a new authenticated CogniteClient:

```python
def get_client():
  from cognite.seismic import CogniteSeismicClient

  def login_and_get_token_supplier(base_url, tenant_id, client_id):
    from msal import PublicClientApplication

    authority = 'https://login.microsoftonline.com/' + tenant_id
    app = PublicClientApplication(client_id=client_id, authority=authority)
    scopes = [base_url + '.default']
    try:
      account = app.get_accounts()[0]
    except IndexError:
      app.acquire_token_interactive(scopes=scopes)
      try:
        account = app.get_accounts()[0]
      except IndexError:
        raise Exception('No accounts returned after Azure AD login. Check permissions?')
    print(f"Logged in as {account['username']}")

    def tokenmaker():
        return app.acquire_token_silent(account=account, scopes=scopes)['access_token']

    return tokenmaker

  base_url = '<base_url>'
  tenant_id = '<tenant_id>'
  client_id = '<client_id>'
  project = '<project>'

  tokenmaker = login_and_get_token_supplier(base_url, tenant_id, client_id)
  return CogniteSeismicClient(base_url=base_url, project=project, oidc_token=tokenmaker)

client = get_client()

```

## Next steps

See the SDK [reference documentation](https://cognite-seismic-sdk.readthedocs-hosted.com/en/latest/cognite.html) to learn more about the Seismic SDK.
