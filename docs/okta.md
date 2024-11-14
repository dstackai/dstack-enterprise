# dstack Enterprise Okta integration

`dstack` Enterprise supports Single Sign-On via [Okta Workforce Identity Cloud](https://www.okta.com/workforce-identity/).
When Okta integration is configured, the `dstack` login page will display the **Sign in with Okta** button.
After signing in with Okta for the first time, a new `dstack` user account is created and linked to the Okta account. Subsequently, users can log in to `dstack` using their Okta account without entering any `dstack`-specific credentials.

The `dstack` Okta integration uses the [OpenID Connect standard](https://developer.okta.com/docs/concepts/oauth-openid/) with the [Authorization Code flow](https://developer.okta.com/docs/guides/implement-grant-type/authcode/main/) as recommended by Okta.

This guide shows how to set up `dstack` Enterprise with Okta.

## Add a private SSO integration

First you need to create a private SSO integration for `dstack` Enterprise in your Okta organization as described in the [Okta docs](https://developer.okta.com/docs/guides/add-private-app/openidconnect/main/).

1. Go to **Applications > Applications** in the Admin Console
2. Click **Create App Integration**.
3. Select the **OpenID Connect** in the **Sign-in method** section.
4. Choose **Web Application** as the **Application type**. Click Next.
5. Enter application settings:
    * **App integration name**: dstack Enterprise
    * **Grant type**: Authorization Code
    * **Sing-in redirect URIs**: `{DSTACK_SERVER_URL}/auth/okta/callback`, e.g. `https://enterprise-example.dstack.ai/auth/okta/callback`
    * **Sign-out redirect URIs**: `{DSTACK_SERVER_URL}`, e.g. `https://enterprise-example.dstack.ai`
    * **Assignments**: Select "Allow everyone in your organization to access" if you have no other preferences.

## Configure Okta on the `dstack` server

To enable Okta Single Sign-On on the `dstack` server, you need to set the following environment variables:
* `DSTACK_OKTA_DOMAIN` - the domain of your Okta organization, e.g. `dev-123456.okta.com`.
* `DSTACK_OKTA_CLIENT_ID` - the Client ID of the `dstack` Enterprise application created in the previous step.
* `DSTACK_OKTA_SECRET` - the Secret of the `dstack` Enterprise application created in the previous step.

Also ensure that you have the `DSTACK_SERVER_URL` set to an URL of the dstack Enterprise installation,
if it's different from the default `http://localhost:3000`, e.g. `https://enterprise-example.dstack.ai`.
