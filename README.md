# Triton Docker CLI helper

This script installs known-good, tested versions of the Docker (now Moby) and Docker Compose CLI tools for use with Triton.

Additionally, this script will automatically configure those tools for use with Triton when you use them, making it easy to switch between Docker on your laptop and Docker on Triton.

### Compatibility and requirements

This is designed and tested for Linux and MacOS X.

This code also requires [the Triton CLI tools](https://docs.joyent.com/public-cloud/api-access/cloudapi) and [a Triton account](https://docs.joyent.com/public-cloud/getting-started) on either the Triton Public Cloud (Joyent public cloud) or in a private cloud powered by Triton.

Use of this software also requires [a Triton profile configured in the Triton CLI tool](https://docs.joyent.com/public-cloud/api-access/cloudapi#configuration).

### Installation

In a terminal window, run the following command:

```bash
curl -o /usr/local/bin/triton-docker https://raw.githubusercontent.com/joyent/triton-docker-cli/master/triton-docker && chmod +x /usr/local/bin/triton-docker && ln -Fs /usr/local/bin/triton-docker /usr/local/bin/triton-compose && ln -Fs /usr/local/bin/triton-docker /usr/local/bin/triton-docker-install
```

That command will copy the `triton-docker` shell script from this repo, and link it as `triton-compose` and `triton-docker-install`.

To complete the installation, run `sudo triton-docker-install` to install the platform-specific versions of the Docker (now Moby) and Docker Compose CLI tools. These versions will not replace any existing Docker or Docker Compose versions you may have installed.

### Usage

Once installed, use `triton-docker` and `triton-compose` in place of 

### Components

In addition to the shell script in this repo, this script will install:

- [Docker (now Moby) 1.12.6](https://github.com/moby/moby/releases/tag/v1.12.6)
- [Docker Compose 1.9.0](https://github.com/docker/compose/releases/tag/1.9.0)
