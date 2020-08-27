---
title: "NGINX Buildpack"
weight: 303
menu:
  main:
    parent: "buildpacks"
---

# NGINX Buildpack

The NGINX CNB supports the installation of the NGINX binary distribution onto
the `$PATH` inside a container. This makes it available to subsequent
buildpacks.

To build a sample app locally with this CNB using the `pack` CLI, run

{{< code/copyable >}}
git clone https://github.com/paketo-buildpacks/samples
cd samples/nginx
pack build my-app --buildpack gcr.io/paketo-buildpacks/nginx:latest
{{< /code/copyable >}}

See [samples](https://github.com/paketo-buildpacks/samples/tree/main/nginx)
for how to run the app.

## Supported dependencies

See [NGINX Buildpack releases](https://github.com/paketo-buildpacks/nginx/releases)
for a full list of dependencies that are used by the buildpack.

## Specifying an NGINX Version

The NGINX CNB allows you to specify a version of NGINX to use during
deployment. This version can be specified in a number of ways, including
through `buildpack.yml`. When specifying a
version of the NGINX engine, you must choose a version that is available
within the buildpack.

Specifying a version of `nginx` is not required. In the case that it is not
specified, the buildpack will provide the default version listed in the release
notes.

### Using buildpack.yml

To configure the buildpack to use NGINX v1.17.9 when deploying your app, for example,
include the values below in your `buildpack.yml` file:

{{< code/copyable >}}
nginx:
  # this allows you to specify a version constaint for the `NGINX` dependency
  # any valid semver constaints (e.g. 1.* and 1.17.*) are also acceptable
  #
  # you can also specify "mainline" or "stable"
  version: 1.17.9
{{< /code/copyable >}}

## Configurations
The NGINX buildpack supports two app configurations:

1. When an `nginx.conf` file **is present** in your app's source code, the
   buildpack will set up an NGINX server with that config.

1. When the `nginx.conf` **is not present** in the app's source code, the
   buildpack simply provides the NGINX dependency to subsequent buildpacks
   without actually setting up a server.

## Custom NGINX configs

The NGINX CNB also supports data driven templates for nginx config. You may use
variables like `{{port}}`, `{{env "FOO"}}` and `{{module "ngx_stream_module"}}`.

See the [NGINX
docs](https://nginx.org/en/docs/beginners_guide.html#conf_structure) for more
information about how to set up an `nginx.conf` file.