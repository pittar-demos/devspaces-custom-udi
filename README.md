# Dev Spaces - Custom UDI Demo

Red Hat OpenShift Dev Spaces is a powerful, full featured IDE experience.  The IDE and tooling run on your OpenShift cluster as a pod in your own namespace, then you access the IDE through your browser.

Red Hat provides a well equiped "Universal Developer Image" (UDI) that contains languages, runtimes, command line utilities, etc... often, this UDI image has everything you need to get your work done.  However, sometimes you need an extra utility or two, and you want to make sure everyone on the team is using the right version.  This is where UDI customization comes in.

## How To Customize UDI?

The Universal Developer Image (UDI) is just a container image that is provided by Red Hat.  This means that the easiest way to customize your development environment and make sure everyone is using the same tools is to simply create your own UDI image based on the one provided by Red Hat.

This requires building a new image, then updating the Dev Spaces configuration to use this new image by default.

## The Containerfile

First, you'll need a `Containerfile` (sometimes called a `Dockerfile`, but I won't be using Docker in this demo...).

```
# Use the official Red Hat UDI as the base
FROM quay.io/devspaces/udi-rhel9:latest

# Switch to root to install packages
USER root

# Option A: Install via package manager
RUN microdnf install -y <package-name>

# Option B: Add a binary manually
RUN curl -Lo /usr/local/bin/my-binary https://example.com && \
    chmod +x /usr/local/bin/my-binary

# Revert to the default 'user' (UID 1001) for security
USER 1001
```

This is a basic example.  For this demo, I'll be adding the following utilities:

* AWS CLI
* Azure CLI
* Helm
* JQ/YQ
* ROSA CLI
* Powershell
* Azure Powershell

Or at least a subset of these... we'll ses how many I get to :)