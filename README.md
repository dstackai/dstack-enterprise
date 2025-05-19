# dstack Enterprise

`dstack` Enterprise extends the [open-source version](https://github.com/dstackai/dstack) of `dstack`
with SSO and other advanced governance features.
It’s fully compatible with the open-source `dstack` CLI and API.

Request a free 60-day trial of `dstack` Enterprise [here](https://calendly.com/dstackai/discovery-call).

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
