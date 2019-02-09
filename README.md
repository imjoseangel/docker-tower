[![](https://images.microbadger.com/badges/image/imjoseangel/ansible-tower.svg)](https://microbadger.com/images/imjoseangel/ansible-tower "Get your own image badge on microbadger.com")

## Ansible Tower Enterprise on Docker

A Docker Ansible Tower image based on `centos 7`.

:rotating_light: **This image is meant for development use only** :rotating_light:

Before you starting the container, run the following command to set up the Docker host. It uses [special privileges](https://docs.docker.com/engine/reference/run/#/runtime-privilege-and-linux-capabilities) to create a cgroup hierarchy 
for `systemd`. We do this in a separate setup step so we can run `systemd` in unprivileged containers.

    docker run --rm --privileged -v /:/host imjoseangel/ansible-tower setup

### Running

A couple of flags are needed to the `docker run` command to make `systemd` play nice with Docker.

Disable seccomp because `systemd` uses system calls that are not allowed by Docker's default seccomp profile:

    --security-opt seccomp=unconfined

CentOS's `systemd` expects `/run` to be a `tmpfs` file system, but it can't mount the file system itself in an unprivileged container:

    --tmpfs /run

`systemd` needs read-only access to the kernel's cgroup hierarchies:

    -v /sys/fs/cgroup:/sys/fs/cgroup:ro

Allocating a pseudo-TTY is not strictly necessary, but it gives us pretty color-coded logs that we can look at with `docker logs`:
   `-t`

### Full Command

```shell
docker run -t -p 443:443 --name tower --security-opt seccomp=unconfined --tmpfs /run -v /sys/fs/cgroup:/sys/fs/cgroup:ro imjoseangel/ansible-tower
```

### Login

**URL:** https://localhost

**Username:** admin

**Password:** password

### License

Copyright © 2019 [imjoseangel](http://imjoseangel.github.com). Licensed under [the MIT license](https://github.com/imjoseangel/docker-tower/blob/master/LICENSE).
