---
pagination_next: null
pagination_prev: null
title: Register an app in Azure AD
---

# Register an app in Azure AD

Follow the steps below to register an app in Azure AD to use with the Python SDK, Jupyter notebooks, or Postman. You can use the same steps to register desktop apps and one-off/short-term scripts. Users can sign in using their browser and use the acquired token in for example Jupyter.

<!--## Register an app in Azure AD to use with the Python SDK/Jupyter or Postman-->

1. Sign in to the [Azure portal](https://portal.azure.com/) as an admin.

1. If you have access to multiple tenants, use the Directory + subscription filter <img  src="/images/shared/azure-portal-directory-subscription-filter.png" alt="" /> in the top menu to select the tenant in which you want to register an application.

1. Search for and select **Azure Active Directory**.

1. Under **Manage**, select **App registrations** > **New registrations**.

1. In the **Register an application** window, enter the app name, and then select **Register**.

1. Specify the **name** and select the supported **account types**.

1. Under **Redirect URI (optional)**, select **Public client/native (mobile & desktop)** and specify the **redirect URI**:

    - **Jupyter/Python SDK**: `http://localhost:53000`.
    - **Postman**: [https://oauth.pstmn.io/v1/callback](https://oauth.pstmn.io/v1/callback).

    <img className="screenshot" src="/images/cdf/access/azure_reg_app_jupyter_sdk.png" alt="Register an application" width="80%"/>

1. Select **Register**.

1. Select **Authentication** to add more **redirect URIs**, and to select **device code flow** (optional). Then select **Save**.

    <img className="screenshot" src="/images/cdf/access/azure_redir_uris_jupyter_sdk.png" alt="Add redirect URIs" width="80%"/>

1. Configure API permissions:

    1. Select **API permissions**. The Microsoft Graph `User.Read` permissions should already be selected.
    1. Select **Add a permission** and in the next screen, under **APIs my organization uses**, select the CDF API, for example `westeurope-1`.
    1. For **Delegated permissions**, select the required permissions for your application, for example, `user_impersonation`. The delegated permissions filter the permissions a user has based on group memberships but don't add any permissions.

       To use the [token inspection endpoint](/api#operation/inspectToken), select `IDENTITY`.

       Learn more about the available permissions [here](https://docs.cognite.com/cdf/access/concepts/access_token_scopes).

    1. Select **Add permissions**.
    1. The API permissions should look similar to this:

     <img className="screenshot" src="/images/cdf/access/azure_api_perms_user_impersionation.png" alt="API permissions" width="x%"/>

    1. Select **Grant admin consent for...** and confirm that you want to make the new list of permissions active.

       <!-- ![Grant admin consent](/images/cdf/access/azure_grant_admin_consent.png 'Grant admin consent') -->

<!-- 1. If you're using client secrets/certificates, select these permissions for **Application permissions**:
       - Group.Read.All -->
