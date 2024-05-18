# Overview of the get started guide

## What is a container?

- A container is a sandboxed process running on a host machine that is isolated from all other processes running on that host machine.

- That isolation leverages kernel namespaces and cgroups, features that have been in Linux for along time.

- Think of a container as an extended version of `chroot`.

## What is image?

- A running container uses an isolated filesystem.

- This isolated filesystem is provided by an image, and the image must contain everything needed to run an application - all dependencies, configurations, scripts, binaries, etc.

- The image also contains other configurations for the container, such as environment variables, a default command to run, and other metadata.
