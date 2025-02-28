# dstack Enterprise Entra ID integration

`dstack` Enterprise supports Single Sign-On via [Microsoft Entra ID](https://www.microsoft.com/en-us/security/business/identity-access/microsoft-entra-id) (formerly Azure AD). When Entra ID integration is configured, the `dstack` login page will display the **Sign in with Microsoft Entra ID** button.

After signing in with Entra ID for the first time, a new `dstack` user account is created and linked to the Entra ID account. Subsequently, users can log in to `dstack` using their Entra ID account without entering any `dstack`-specific credentials.

The `dstack` Entra ID integration uses the [Microsoft Authentication Library (MSAL)](https://learn.microsoft.com/en-us/azure/active-directory/develop/msal-overview) with the OAuth 2.0 Authorization Code flow.

This guide shows how to set up `dstack` Enterprise with Entra ID.

## Register an application in Entra ID

First, you need to register an application for `dstack` Enterprise in your Microsoft Entra ID tenant as described in the [Microsoft documentation](https://learn.microsoft.com/en-us/azure/active-directory/develop/quickstart-register-app).

1. Sign in to the [Azure portal](https://portal.azure.com).
2. Search for and select **Microsoft Entra ID**.
3. Under **Manage**, select **App registrations** > **New registration**.
4. Enter application settings:
    * **Name**: dstack Enterprise
    * **Supported account types**: Accounts in this organizational directory only (Single tenant)
    * **Redirect URI**: Select **Web** and enter `{DSTACK_SERVER_URL}/auth/entra/callback`, e.g., `https://enterprise-example.dstack.ai/auth/entra/callback`.
5. Click **Register**.
6. Under **Manage**, select **Certificates & secrets** > **New client secret**.
7. Add a description for the secret, choose an expiration time, and click **Add**.
8. Save the secret value (you won't be able to see it again).

## Configure Entra ID on the `dstack` server

To enable Entra ID Single Sign-On on the `dstack` server, you need to set the following environment variables:

* `DSTACK_ENTRA_TENANT_ID` - the Directory (tenant) ID of your Entra ID organization, found in the Azure portal under Microsoft Entra ID > Overview.
* `DSTACK_ENTRA_CLIENT_ID` - the Application (client) ID of the `dstack` Enterprise application created in the previous step.
* `DSTACK_ENTRA_CLIENT_SECRET` - the Client Secret of the `dstack` Enterprise application created in the previous step.

Also, ensure that you have `DSTACK_SERVER_URL` set to the URL of the `dstack` Enterprise installation if it's different from the default `http://localhost:3000`, e.g., `https://enterprise-example.dstack.ai`.

