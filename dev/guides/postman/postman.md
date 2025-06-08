---
pagination_next: null
pagination_prev: null
---

# Postman

We recommend downloading, installing, and using **[Postman](https://www.getpostman.com/)** to test API requests and verify responses.

## Set up Postman using OpenID Connect

## Before you start

To use the different grant types (Implicit, Authorization code (With PKCE)) with your requests in Postman, you need to grant access to a multi-tenant app in Azure AD to use CDF with Postman. To grant access, you must be an **Azure AD tenant administrator**.

Follow the steps in [How to register Cognite API](https://docs.cognite.com/cdf/access/guides/configure_cdf_azure_oidc#step-11-permit-the-cognite-api-to-access-user-profiles-in-azure-ad) to register the app. When you have registered the app, you can sign in with your Azure AD credentials.

Before you set up authorization in Postman, set up your **Azure Active Directory**:

   - Go to **Azure Active Directory**, you'll find your **Tenant ID** and **Application (client) ID** in the overview page.
   
      <img className="screenshot" src="/images/cdf/dev/quickstart/aad-tenant-id.png" alt="Tenant ID" width="500px"/>

   - In the **App registrations** section, create a **New client secret** under **Certificates & secrets** in the left menu. Select **+ New client secret**, enter a description, an expiry period, and select **Add**.

      <img class="screenshot" src="https://apps-cdn.cogniteapp.com/@cognite/docs-portal-images/1.0.0/images/cdf/dev/quickstart/add-client-secret.png" alt="Client secret" width="500px"/>

   - You must add the **Redirect URL** in your Azure AD application.

## Step 1: Import your Postman collection

Download the [**Cognite OpenAPI specification**](https://api-docs.cognite.com).

1. In Postman, select **Import** and drag the file to the import modal. In ** View Import Settings **, you can choose how to import your API and manage the import settings.

1. In the **Import Settings**, set the **Folder organization** to **Tags**, select **Enable optional parameters** to turn it off, and select **Always inherit authentication** to turn it on. Select **Import** to import this data.

   <img className="screenshot" src="/images/cdf/dev/quickstart/how-to-import-api.png" alt="Import Cognite API" width="75%"/>

1. Select **Continue** > **Import** to import the collection.

## Step 2: Set up environment variables

1. To create a new environment, navigate to **Environments** on the left sidebar. Select **+ Create new Environment** and give it a name.

   <img className="screenshot" src="/images/cdf/dev/quickstart/create-environment.png" alt="Create environment" width="350px"/>

2. Add the variables:

   - **tenant-id**: This is your Directory (tenant) ID.

 :::note
 We recommend that you work with the current value of a variable to prevent sharing sensitive and confidential information with your team.
 :::

   - **token**: Use OAuth 2.0 to generate a new token. It will populate automatically, so you can leave it blank as an environment variable.
   
   - **baseUrl**: When you import the collection, the baseUrl is set to `https://{{cluster}}.cognitedata.com/api/v1/projects/{{project}}`. *cluster* is where your CDF instance is installed. If you don't know the cluster name, contact [Cognite support](mailto:support@cognite.com).
   
 For Open Industrial Data, the cluster is `api`.

   - **project**: This is your CDF project name.

## Step 3: Update authorization

 With OAuth 2.0, you retrieve an API access token and use it to authenticate future API requests.

 To use OAuth 2.0:
   - Navigate to the **Authorization** tab in the collection overview, select **OAuth 2.0** as **Type** and **Request Headers** as **Add authorization data to**.
   - To request an access token, fill out the fields in the **Configure New Token** section and select **Get New Access Token**.
   - Input your client application and authorization details from the service provider to test the authenticated requests.
   - Input the details depending on the OAuth 2.0 grant type you want to use - [**Implicit**](#implicit), [**Authorization Code (With PKCE)**](#authorization-code-with-pkce), Authorization Code or Client credentials.

 To understand how to register and configure the application and its authentication flows, see [Configure applications and the authentication flows](https://docs.cognite.com/cdf/access/guides/configure_apps_oidc). The following section describes the authentication flow for each of the grant types:

### Implicit

1. Select **Configure New Token** and specify these configuration options:

   - Enter a **Token Name**.
   - Select the **Grant Type** as **Implicit**.
   - Input the **Callback URL** as `https://postman.cogniteapp.com/loggedin`.

 :::info NOTE
 If you select the checkbox **Authorise using browser**, the Callback URL auto-populates. Otherwise, you can input the Callback URL. Once your application is authorized, you will be redirected to the **Callback URL**.
 :::

   - Enter the **Auth URL** as `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/authorize`. Replace the **tenant-id**.
   - Input the **Client ID** as `https://postman.cogniteapp.com`.
   - The **Scope** is `https://{{cluster}}.cognitedata.com/` + `default`, `user_impersonation`, `DATA.VIEW`, or `IDENTITY`.
   
      `user_impersonation` grants all permissions to the user assigned to access the API. The `DATA.VIEW` scope grants read-only access to data in CDF, for example, to view files, time series, RAW, and other CDF resources. To learn more about CDF's scopes, see the different [Access token scopes](https://docs.cognite.com/cdf/access/concepts/access_token_scopes/).

 :::tip
 When using a scope for the first time, the admin must define it explicitly and consent to use it for the authorization process.
 :::
         
   - Select **Client Authentication** as **Send as Basic Auth header**.

      <img className="screenshot" src="https://apps-cdn.cogniteapp.com/@cognite/docs-portal-images/1.0.0/images/cdf/dev/quickstart/implicit-grant-oidc.png" alt="OAuth 2.0 implicit" width="750px"/>

2. Select **Get New Access Token** > **Proceed** > **Use Token**.

You have configured a new token using the **Implicit** grant type.

### Authorization Code (With PKCE)

1. Select **Configure New Token** and specify the configuration options:

   - Enter a **Token Name**.
   - Select the **Grant Type** as **Authorization Code (With PKCE)**.
   - Input the **Callback URL** as `https://oauth.pstmn.io/v1/callback`.
   
   - Enter the **Auth URL** as `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/authorize`. Replace the **tenant-id**.
   - Enter the **Access Token URL** as `https://login.microsoftonline.com/{{tenant-id}}/oauth2/v2.0/token`. Replace the **tenant-id**.
   - Input the **Client ID** and **Client Secret**.

   - You can choose to use `SHA-256` or `Plain` algorithms to generate the **Code Challenge Method**.
   **Code Verifier** is an optional 43-128-character string that connects the authorization request to the token request. If left blank, the field is automatically generated. 

   - The **Scope** is `https://{{cluster}}.cognitedata.com/` + `default`, `user_impersonation`, `DATA.VIEW`, or `IDENTITY`.

   - Input a random value for **State** to prevent cross-site request forgery.
   - Select **Client Authentication** as **Send as Basic Auth header**.

      <img className="screenshot" src="https://apps-cdn.cogniteapp.com/@cognite/docs-portal-images/1.0.0/images/cdf/dev/quickstart/authorization-code-pkce.png" alt="OAuth 2.0 Authorization code PKCE" height="550px" width="500px"/>

2. Select **Get New Access Token** > **Proceed** > **Use Token**.

You have configured a new token using **Authorization Code (With PKCE)** grant type.

You are now ready to use Postman with OIDC as the authentication method.