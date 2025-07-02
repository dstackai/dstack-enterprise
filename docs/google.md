# dstack Enterprise Google integration

`dstack` Enterprise supports Single Sign-On via [Google](https://developers.google.com/identity/gsi/web/guides/overview). When Google integration is configured, the `dstack` login page will display the **Sign in with Google** button.

After signing in with Google for the first time, a new `dstack` user account is created and linked to the Google account. Subsequently, users can log in to `dstack` using their Google account without entering any `dstack`-specific credentials.

> [!NOTE]
> 
> `dstack` automatically links Google accounts to existing `dstack` users instead of creating new users if their emails match. Global admins should assign Google emails to `dstack` users if automatic linking is desirable.

The `dstack` Google integration uses the OAuth 2.0 and OpenID Connect (OIDC) standards.

## Configure the GCP

> [!IMPORTANT]
> While user accounts are managed in Google Workspace, [Google Cloud Platform (GCP) is used](https://developers.google.com/workspace/guides/auth-overview) to register applications that can use those accounts for authentication. This one-time setup in GCP is required to obtain the OAuth 2.0 credentials (**Client ID** and **Client Secret**) for `dstack` Enterprise.

1. [Create a GCP Project](https://developers.google.com/workspace/guides/create-project)
2. [Configure the OAuth Consent Screen](https://developers.google.com/workspace/guides/configure-oauth-consent)
   1. In the In the GCP Console,  Console, navigate to **APIs & Services > OAuth consent screen**
   2. Fill in the required fields (**App name**, **User support email**, etc.).
   3. Choose **Internal** as the **User Type**.
3. [Create OAuth 2.0 Credentials](https://developers.google.com/workspace/guides/create-credentials)
   1. In the GCP Console, navigate to **APIs & Services > Credentials**
   2. Click **Create OAuth Client**
   3. For **Application type**, select **Web application**

   4. Enter application settings:
    * **Name**: dstack Enterprise
    * **Authorized redirect URIs**: Add a URI in the format `{DSTACK_SERVER_URL}/auth/google/callback`, e.g., `https://enterprise-example.dstack.ai/auth/google/callback`.
   5. Click `Create`.

## Configure Google on the `dstack` server

To enable Google Single Sign-On on the `dstack` server, you need to set the following environment variables:
* `DSTACK_GOOGLE_PROJECT_ID` - your GCP project ID, e.g. `my-dstack-enterprise-project`.
* `DSTACK_GOOGLE_CLIENT_ID` - the **Client ID** of the `dstack` Enterprise application created in the previous step.
* `DSTACK_GOOGLE_CLIENT_SECRET` - the **Client secret** of the `dstack` Enterprise application created in the previous step.

Also ensure that you have the `DSTACK_SERVER_URL` set to an URL of the dstack Enterprise installation,
if it's different from the default `http://localhost:3000`, e.g. `https://enterprise-example.dstack.ai`.
