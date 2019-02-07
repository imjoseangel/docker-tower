FROM centos:7

LABEL maintainer imjoseangel

ENV container docker
ENV ANSIBLE_TOWER_VER 3.4.1-1

RUN yum install -y deltarpm epel-release
RUN yum upgrade -y

# Don't start any optional services except for the few we need.
RUN find /etc/systemd/system \
    /lib/systemd/system \
    -path '*.wants/*' \
    -not -name '*journald*' \
    -not -name '*systemd-tmpfiles*' \
    -not -name '*systemd-user-sessions*' \
    -exec rm \{} \;

RUN systemctl set-default multi-user.target

COPY setup /sbin/

# create /var/log/tower
RUN mkdir -p /var/log/tower

# Download & extract Tower tarball
ADD http://releases.ansible.com/ansible-tower/setup/ansible-tower-setup-${ANSIBLE_TOWER_VER}.tar.gz /opt/ansible-tower-setup-${ANSIBLE_TOWER_VER}.tar.gz
RUN tar xvzf /opt/ansible-tower-setup-${ANSIBLE_TOWER_VER}.tar.gz -C /opt \
    && rm -f /opt/ansible-tower-setup-${ANSIBLE_TOWER_VER}.tar.gz

ADD inventory /opt/ansible-tower-setup-${ANSIBLE_TOWER_VER}/inventory

EXPOSE 443

STOPSIGNAL SIGRTMIN+3

# Workaround for docker/docker#27202, technique based on comments from docker/docker#9212
CMD [ "/bin/bash", "-c", "exec /sbin/init --log-target=journal 3>&1" ]