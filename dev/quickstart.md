---
Title: Development Quickstart
Description: Get started with Cognite Data Fusion (CDF) API using Postman and learn how to authenticate your requests with OpenID Connect (OIDC).
---

Cognite Data Fusion (CDF) authenticates your API requests to a **[project](/api#tag/Projects)** using OpenID Connect (OIDC). Get up and running with the API libraries and start working with CDF in a few simple steps.

The recommendation is to download, install, and use **[Postman](https://www.getpostman.com/)** to test API requests and verify responses.

## Prerequisites

To use the **Implicit** grant type with your requests in Postman, you need to grant access to a multi-tenant app in Azure AD to use CDF with Postman. To grant access, you need to be an Azure AD tenant administrator.

Follow the steps in [How to register Cognite API](https://docs.cognite.com/cdf/access/guides/configure_cdf_azure_oidc#step-11-permit-the-cognite-api-to-access-user-profiles-in-azure-ad) to register the app.

When you have registered the app, you will be able to sign in with your Azure AD credentials.

## Step 1: Import your Postman collection

1. In Postman, select **Import** > **Link** and enter the URL to import your latest API V1 Postman collection:
`https://storage.googleapis.com/cognite-postman-collections/v1.json`

   <img className="screenshot" src="/images/cdf/dev/quickstart/import-collection-link.png" alt="Import collection"/>

1. Select **Continue** > **Import** to import the collection.

## Step 2: Set up environment variables

1. To create a new environment, navigate to **Environments** on the left sidebar. Click **+ Create new Environment** and give it a name.

   <img className="screenshot" src="/images/cdf/dev/quickstart/create-environment.png" alt="Create environment" width="350px"/>

2. Add the variables:

   - **tenant-id**: This is your Directory (tenant) ID. To find the tenant ID, go to your Azure Active Directory. You can find your Tenant ID on the Overview page.

     <img className="screenshot" src="/images/cdf/dev/quickstart/aad-tenant-id.png" alt="Tenant ID" width="500px"/>

   :::info NOTE
   The recommendation is to work with the current value of a variable to prevent sharing sensitive and confidential information with your team.
   :::

   - **token**: Using OAuth 2.0, we'll generate a new token. It will populate automatically, so you will leave it blank as an environment variable.

   - **baseURL**: You can find your baseURL from the CDF project. Navigate to your CDF project. Under **Manage & Configure** > **Manage access**, select **Open ID connect** tab. The URL in the audience field is the baseURL.

     <img className="screenshot" src="/images/cdf/dev/quickstart/baseurl-cdf.png" alt="Base URL" width="450px"/>

   - **project**: This is your CDF project name.

     <img className="screenshot" src="/images/cdf/dev/quickstart/project-name-cdf.png" alt="Project name" width="350px"/>

3. Click **Save** to save the environment variables you have added.

4. If necessary, select your newly created environment in the dropdown menu in the top right corner.

<img className="screenshot" src="/images/cdf/dev/quickstart/select-environment.png" alt="List all assets" width="300px"/>

## Step 3: Update authorization

1. To update the authorization, navigate to the **Authorization** tab in the collection overview.

2. Select **OAuth 2.0** as Type and **Request Headers** as Add auth data to.

3. Select **Configure New Token** and specify these configuration options:

   - Enter a **Token Name**.
   - Select the **Grant Type** as **Implicit**.
   - Input the **Callback URL** as `https://postman.cogniteapp.com/loggedin`.

     :::info NOTE
     If you don't select the checkbox **Authorise using browser**, you can input the Callback URL. Otherwise, the Callback URL gets auto-populated on selection. You will be redirected to the **Callback URL** once your application is authorized.
     :::

   - Enter the **Auth URL** as `https://login.microsoftonline.com/$tenant-id/oauth2/v2.0/authorize`. Replace the **tenant-id** obtained from the previous step.
   - Input the **Client ID** as `https://postman.cogniteapp.com`.
   - The **Scope** is `$baseUrl/$scope`, where **$baseUrl** is as above in 2.2, and **$scope** is `IDENTITY`, `DATA.VIEW`, `user_impersonation` etc.

   :::tip tip
   While using a scope for the first time, the admin has to define the scope explicitly. The admin must then consent to use this scope for the authorization process.
   :::

   `User_impersonation` grants all permissions to the user assigned to access the API. The `DATA.VIEW` scope grants read-only access to data in CDF, for example, to view files, time series, RAW, and other CDF resources. To know more about CDF's scopes, see the different [Access token scopes](https://docs.cognite.com/cdf/access/concepts/access_token_scopes).

   - Select **Client Authentication** as **Send as Basic Auth header**.

      <img className="screenshot" src="/images/cdf/dev/quickstart/implicit-grant-oidc.png" alt="OAuth 2.0 implicit"/>

4. Select **Get New Access Token** > **Proceed** > **Use Token**.

You are now ready to use Postman with OIDC as the authentication method.

## Step 4: Send your API request

1. Make a test API request to check that your integration is working correctly:

    - Click the **Cognite API** collection in the left-hand menu, and select **Assets** > **List assets**.

       <img className="screenshot" src="/images/cdf/dev/quickstart/cogniteapi-collection.png" alt="List assets" width="350px"/>

    - Click **Send**.

Cognite Data Fusion returns a list of all [asset objects](/api#tag/Assets) in response to your API request. For the [Open Industrial Data Project](https://www.youtube.com/watch?v=g4DAAo55IS8) the JSON results look similar to this:

```json
{
    "nextCursor": "8ZiApWzGe5RnTAE1N5SABLDNv7GKkUGiVUyUjzNsDvM",
    "items": [
        {
            "name": "23-TE-96116-04",
            "parentId": 3117826349444493,
            "description": "VRD - PH 1STSTGGEAR THRUST BRG OUT",
            "metadata": {
                "ELC_STATUS_ID": "1211",
                "RES_ID": "525283",
                "SOURCE_DB": "workmate",
                "SOURCE_TABLE": "wmate_dba.wmt_tag",
                "WMT_AREA_ID": "1600",
                "WMT_CATEGORY_ID": "1116",
                "WMT_CONTRACTOR_ID": "1686",
                "WMT_FUNC_CODE_ID": "4564",
                "WMT_LOCATION_ID": "1004",
                "WMT_PO_ID": "8309",
                "WMT_SAFETYCRITICALELEMENT_ID": "1060",
                "WMT_SYSTEM_ID": "4440",
                "WMT_TAG_CREATED_DATE": "2009-06-26 15:36:37",
                "WMT_TAG_CRITICALLINE": "N",
                "WMT_TAG_DESC": "VRD - PH 1STSTGGEAR THRUST BRG OUT",
                "WMT_TAG_GLOBALID": "1000000000681024",
                "WMT_TAG_HISTORYREQUIRED": "Y",
                "WMT_TAG_ID": "346434",
                "WMT_TAG_ID_ANCESTOR": "345637",
                "WMT_TAG_ISACTIVE": "1",
                "WMT_TAG_ISOWNEDBYPROJECT": "0",
                "WMT_TAG_LOOP": "96116",
                "WMT_TAG_MAINID": "681760",
                "WMT_TAG_NAME": "23-TE-96116-04",
                "WMT_TAG_UPDATED_BY": "8137",
                "WMT_TAG_UPDATED_DATE": "2014-07-11 09:25:15"
            },
            "id": 702630644612,
            "createdTime": 0,
            "lastUpdatedTime": 0,
            "rootId": 6687602007296940
        },
...
```

Once you have successfully made an API request, you’re ready to begin interacting with the Cognite Data Fusion API through the Software Development Kits (SDKs).

<!-- Note that this headline is linked to from docs/dev/index.mdx, so if you reword or renumber it, you need to update that link. -->
## Step 5: Install a Software Development Kit (SDK)

We provide official Software Development Kits (SDKs) for [Python](/sdks/python/) and [JavaScript](/sdks/js/) with libraries that interact with the Cognite Data Fusion API.

### The Python Software Development Kit (SDK)

The Cognite Python SDK requires Python 3.5+ and provides access to the Cognite Data Fusion API from applications written in the Python language. See the [Cognite Python SDK Documentation](/sdks/python/) for more details.

:::info
To download and install Python, visit [Python.org](https://python.org/).
:::

To install the Cognite Python library:

```sh
pip install cognite-sdk
```

### The JavaScript Software Development Kit (SDK)

The Javascript SDK provides access to the Cognite Data Fusion API from applications written in client-side or server-side JavaScript. The library supports authentication through API keys (for server-side applications) and bearer tokens (for web applications). See the [Cognite JavaScript documentation](/sdks/js/) for more details.

Install the Cognite JavaScript library:

```bash npm2yarn
npm install @cognite/sdk --save
```

## Next steps

- [Quickstart tutorial with Python SDK](/sdks/python/)
- [Quickstart tutorial with JavaScript SDK](/sdks/js/)
- Visualize and explore data through powerful dashboards using the [Cognite Grafana plug-in](https://docs.cognite.com/cdf/dashboards/guides/grafana/getting_started) and [PowerBI](https://docs.cognite.com/cdf/dashboards/guides/powerbi/getting_started)
- [Charts](https://docs.cognite.com/cdf/charts/)
- [InField](https://docs.cognite.com/infield/)
