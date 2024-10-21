# Triton Docker CLI helper

**NOTE:** You almost certainly should not use this repo anymore.

## Set up `docker` to use Triton without using the script in this repo

With modern versions of `docker` and Triton the script in this repo is no longer
necessary. Before you begin, make sure you have installed and set up the
[`triton`][1] cli.

First, generate your client certificate.

```sh
triton profile docker-setup
```

Once that's done, in any shell you want to use `docker` with Triton, run the
following. Most people place this in their shell rc file (`.bashrc`, `.zshrc`,
etc.).

```sh
eval "$(triton env -d)"
```

Once this is done, just use `docker` as you normally would.

See the [`node-triton`][1] documentation for additional details and command line
arguments available.

[1]: /TritonDataCenter/node-triton/

----

## Information below is for historical record

----

This script installs known good, tested versions of the Docker (now Moby) and Docker Compose CLI tools for use with Triton.

Additionally, this script will automatically configure those tools for use with Triton when you use them, making it easy to switch between Docker on your laptop and Docker on Triton.

### Compatibility and requirements

This is designed and tested for Linux and MacOS X.

This code also requires [the Triton CLI tools](https://docs.joyent.com/public-cloud/api-access/cloudapi) and [a Triton account](https://docs.joyent.com/public-cloud/getting-started) on either Triton public cloud (Joyent public cloud) or in a private cloud powered by Triton.

Use of this software also requires [a Triton profile configured in the Triton CLI tool](https://docs.joyent.com/public-cloud/api-access/cloudapi#configuration).

### Installation

In a terminal window, run the following command:

```bash
sudo bash -c 'curl -o /usr/local/bin/triton-docker https://raw.githubusercontent.com/joyent/triton-docker-cli/master/triton-docker && chmod +x /usr/local/bin/triton-docker && ln -Fs /usr/local/bin/triton-docker /usr/local/bin/triton-compose && ln -Fs /usr/local/bin/triton-docker /usr/local/bin/triton-docker-install'
```

That command will copy the `triton-docker` shell script from this repo, and link it as `triton-compose` and `triton-docker-install`.

To complete the installation, run `sudo triton-docker-install` to install the platform-specific versions of the Docker (now Moby) and Docker Compose CLI tools. These versions will not replace any existing Docker or Docker Compose versions you may have installed.

### Usage

Once installed, use `triton-docker` and `triton-compose` in place of `docker` and `docker-compose` when interacting with the Triton Elastic Docker Host.

Start a Docker container running Nginx container on Triton:

```bash
$ triton-docker run -d -p 80 --name webserver nginx
Executing in 'us-sw-1' (default; use `triton profile set <profile name>` to change) at 03:11:11 PM
d5cae48b0072610ecc67f6aecb3115f9fadff59b2151694a963084dad40e5d85
$
```

Start [all the containers to run WordPress](https://github.com/autopilotpattern/wordpress) via Docker Compose on Triton:

```bash
$ triton-compose up -d
Executing in 'us-sw-1' (default; use `triton profile set <profile name>` to change) at 03:15:56 PM
Creating wp_wordpress_1
Creating wp_nginx_1
Creating wp_nfs_1
Creating wp_memcached_1
Creating wp_prometheus_1
Creating wp_mysql_1
Creating wp_consul_1
$
```

More about:

- [Docker commands on Triton](https://www.joyent.com/blog/docker-commands-on-triton)
- [Docker Compose on Triton](https://www.joyent.com/blog/using-docker-compose)
- [Optimizing your Docker operations for Triton](https://www.joyent.com/blog/optimizing-docker-on-triton)

### Triton Container Name Service helpers

This will also set some helpful environment variables for using [Triton Container Name Service (CNS)](https://docs.joyent.com/public-cloud/network/cns):

- `TRITON_CNS_SEARCH_DOMAIN_PUBLIC`
- `TRITON_CNS_SEARCH_DOMAIN_PRIVATE`

Those vars will be automatically set with values appropriate for use in Triton Public Cloud:

```
TRITON_CNS_SEARCH_DOMAIN_PRIVATE=a42e7881-89d2-459e-bc0b-e9af0bca409a.us-east-3.cns.joyent.com
TRITON_CNS_SEARCH_DOMAIN_PUBLIC=a42e7881-89d2-459e-bc0b-e9af0bca409a.us-east-3.triton.zone
```

#### Notes and cautions about CNS env vars

- The mechanism that sets these environment vars only works for Triton Public Cloud. To use Triton Docker CLI with private cloud implementations of Triton, please set the `TRITON_CNS_SEARCH_DOMAIN_PUBLIC` and `TRITON_CNS_SEARCH_DOMAIN_PRIVATE` vars manually
- These environment variables will be removed following the completion of [CNS-164](https://smartos.org/bugview/CNS-164) and [DOCKER-898](https://smartos.org/bugview/DOCKER-898), which will automatically set the DNS search domain to these values
- [CNS-164](https://smartos.org/bugview/CNS-164) and [DOCKER-898](https://smartos.org/bugview/DOCKER-898) will also work on private cloud implementations of Triton, solving the problem for everybody

### Components

In addition to the shell script in this repo, this script will install:

- [Docker (now Moby) 1.12.6](https://github.com/moby/moby/releases/tag/v1.12.6)
- [Docker Compose 1.9.0](https://github.com/docker/compose/releases/tag/1.9.0)
