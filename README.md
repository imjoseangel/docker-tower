[![Get your own image badge on microbadger.com](https://images.microbadger.com/badges/image/imjoseangel/ansible-tower.svg)](https://microbadger.com/images/imjoseangel/ansible-tower) "Get your own image badge on microbadger.com")

## Ansible Tower Enterprise on Docker

A Docker Ansible Tower image.

:rotating_light: **This image is meant for development use only** :rotating_light:

### Running

Run Ansible Tower with a random port:

```shell
docker run -d -P --name tower imjoseangel/ansible-tower
```

or map to exposed port 443:

```shell
docker run -d -p 443:443 --name tower imjoseangel/ansible-tower
```

To include certificate and license on container creation:

```shell
docker run -t -d -v ~/certs:/certs -p 443:443 -e SERVER_NAME=localhost  ansible-tower
```

To persist Ansible Tower database, create a data container:

```shell
docker create -v /var/lib/postgresql/9.6/main --name tower-data imjoseangel/ansible-tower /bin/true
docker run -d -p 443:443 --name tower --volumes-from tower-data imjoseangel/ansible-tower
```

or use create a Docker Volume on the host:

```shell
docker run -d -p 443:443 -v pgdata:/var/lib/postgresql/9.6/main --name ansible-tower imjoseangel/ansible-tower
```

If you want to persist any Ansible project data saved at `/var/lib/awx/projects` directory, create a Docker Volume on using the command below:

```shell
docker run -d -p 443:443 -v ~/ansible_projects:/var/lib/awx/projects --name ansible-tower imjoseangel/ansible-tower
```

Allocating a pseudo-TTY is not strictly necessary, but it gives us pretty color-coded logs that we can look at with `docker logs`:
   `-t`

### Full Command

```shell
docker run -t -d -v ~/certs:/certs -v ~/pgdata:/var/lib/postgresql/9.4/main -p 443:443 -e SERVER_NAME=myserver.domain.com -h myserver.domain.com --name tower imjoseangel/ansible-tower
```

### Certificates and License

The ansible-tower Docker image uses a generic certificate generated for www.ansible.com by the Ansible Tower setup
program. If you generate your own certificate, it will be copied into /etc/tower by the entrypoint script if a volume
is mapped to /certs in the container, e.g:

* **/certs/tower.cert** -> /etc/tower/tower.cert
* **/certs/tower.key** -> /etc/tower/tower.key

The environment variable SERVER_NAME should match the common name of the generated certificate and will be used to update
the nginx configuration file.

A license file can also be included similar to the certificates by renaming your Ansible Tower license file to **license** and
placing it in your local, mapped volume. The entrypoint script checks for the license file seperately and does not depend
on the certificates.

* **/certs/license** -> /etc/tower/license

The license file can also be uploaded on first login to the Ansible Tower web interface.

### Login

**URL:** `https://localhost`

**Username:** admin

**Password:** password

### License

Copyright © 2019 [imjoseangel](http://imjoseangel.github.com). Licensed under [the MIT license](https://github.com/imjoseangel/docker-tower/blob/master/LICENSE).
