# dstack Enterprise

`dstack` Enterprise extends the [open-source version](https://github.com/dstackai/dstack) of `dstack`
with enterprise features such as a web user interface, advanced team management, and more.
It’s fully compatible with the open-source `dstack` CLI and API.

Request a free 60-day trial of `dstack` Enterprise [here](`dstack` Enterprise).

## Installation

### Getting the image

Once you're given access to `dstack` Enterprise, you can pull the image as follows:

```shell
echo $DSTACK_ENTERPRISE_TOKEN | docker login ghcr.io -u $DSTACK_ENTERPRISE_USERNAME --password-stdin
Login Succeeded
docker pull ghcr.io/dstackai/dstack-enterprise:latest
```

And, run the image:

```shell
docker run -it -p 3000:3000 -v \
  $HOME/.dstack-enterprise/server/:/root/.dstack/server \
ghcr.io/dstackai/dstack-enterprise:latest

The admin user token is "bbae0f28-d3dd-4820-bf61-8f4bb40815da"
The dstack server is running at http://localhost:3000
```

> [!NOTE]
> Replace `latest` with a specific version of `dstack` Enterprise. See [versions](https://github.com/dstackai/dstack-enterprise/releases).

## Versioning

The version format for `dstack` Enterprise is `<dstack version>-<enterpise build>`. Examples: `0.18.2-v1`.
The first part corresponds to the open-source dstack version and CLI, while the second part is the enterprise build number.

The history of `dstack` Enterprise releases can be found in the [`dstackai/dstack-enterprise`](https://github.com/dstackai/dstack-enterprise/releases) repo.

### Backward compatibility

`dstack` is backward compatible within its major version. For example, if `dstack` Enterprise
is `0.18.1-v1`, the version of CLI or API must start with `0.18`.

## Configuring environment variables

Here's the list of environment variables which you can override:

* `DSTACK_SERVER_DIR` – (Optional) The path to the directory where the dstack server stores the state. Defaults to `/root/.dstack/server`.
* `DSTACK_SERVER_ADMIN_TOKEN` – (Optional) The default token of the admin user. By default, it's generated randomly at the first startup.

## Configuring backends

When starting `dstack` Enterprise, ensure the `$DSTACK_SERVER_DIR/config.yml` file
configures a backend for each cloud account that you'd like to use.

See the [server/config.yml reference](https://dstack.ai/docs/reference/server/config.yml.md#examples)
for details on how to configure backends for all supported cloud providers.

## Updating `dstack` Enterprise

Updating `dstack` Enterprise to a newer version requires re-deploying it using the newer Docker image tag. 

The server can be restarted without concern for submitted jobs, running instances, or gateways.
Upon restart, it will resume tracking their states. 

> [!NOTE]
> Currently, only one instance of dstack Enterprise is allowed to run at a time. During server restart, the CLI or API may temporarily stop working.

## Persisting state

`dstack` stores its state in the ``$DSTACK_SERVER_DIR/data`` folder (by default `/root/.dstack/server/data`) with SQLite.

### Persisting state with Litestream

If required, `dstack` can automatically replicate its state to a cloud object storage via [Litestream](https://litestream.io/getting-started/) by configuring the
necessary environment variables.

* `LITESTREAM_REPLICA_URL` - The url of the cloud object storage. Examples: `s3://<bucket name>/<path>`, `gcs://<bucket name>/<path>`, `abs://<storage account>@<container name>/<path>`, etc.

#### AWS S3

To persist state into an AWS S3 bucket, provide the following environment variables:

* `AWS_ACCESS_KEY_ID` - The AWS access key ID
* `AWS_SECRET_ACCESS_KEY` -  The AWS secret access key

#### GCP Storage

To persist state into an AWS S3 bucket, provide one of the following environment variables:

* `GOOGLE_APPLICATION_CREDENTIALS` - The path to the GCP service account key JSON file
* `GOOGLE_APPLICATION_CREDENTIALS_JSON` - The GCP service account key JSON

#### Azure Blob Storage

To persist state into an Azure blog storage, provide the following environment variable.

* `LITESTREAM_AZURE_ACCOUNT_KEY` - The Azure storage account key

An alternative to configuring explicit credentials is to provide the default cloud credentials within the container.

> [!NOTE]
> The use of Litestream requires that only one instance of the dstack server is running at a time.

## Configuring backups

It’s recommended to regularly back up the state. If the state is being persisted with Litestream, you can back up data using the standard mechanism provided by the cloud object storage.

It’s recommended to back up the state before every update to a newer minor version (e.g. from 0.17.1-v1 to 0.18.0-v3).

> [!NOTE]
> If the state is being persisted with Litestream, before restoring a backup, it’s important to undeploy dstack Enterprise first.

## Managing users

It’s possible to create users and manage their permissions via UI. Each user can be configured with a global role: `User` or `Admin`. The global Admin role allows for managing any users and projects.

In addition to the global role, any user can be granted a project role: `User` or `Admin`. The project `User` role allows for the management of runs. The project Admin role permits managing the members of the project (other users who have permissions for this project).

### Configuring the admin token

During deployment, it’s important to configure `DSTACK_SERVER_ADMIN_TOKEN`. This token can be used by the admin to manage other users and projects. If the environment variable is not set, it's generated randomly at the first startup.

### Authentication

Currently, `dstack` Enterprise allows authentication using personal access tokens. Each user has their personal token, which can be found or changed in the user's account settings. This token must be used for authentication to access both the UI, CLI, or API.

### Managing projects

Projects group and isolate users and resources. It's possible to create multiple projects, configure different clouds for each project, and grant different users access to these projects.

Once a project is configured, a user with `Admin` role can manage the project members.