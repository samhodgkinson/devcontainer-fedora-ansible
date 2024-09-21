FROM fedora:40
LABEL maintainer="Sam Hodgkinson"

ARG DEBIAN_FRONTEND=noninteractive

ARG ANSIBLE_VERSION=2.17

# Remove unnecessary getty and udev targets that result in high CPU usage when using
# multiple containers with Molecule (https://github.com/ansible/molecule/issues/1104)
RUN rm -f /lib/systemd/system/systemd*udev* \
  && rm -f /lib/systemd/system/getty.target

RUN dnf -y update \
  && dnf -y install \
  dnf-utils \
  @development-tools \
  glibc-langpack-en \
  libffi-devel \
  openssl-devel \
  libyaml-devel \
  python3-devel \
  python3-setuptools \
  python3-pip \
  python3-pyyaml \
  rsyslog \
  systemd \
  cronie \
  sudo \
  iproute \
  openssh-clients \
  iputils \
  git \
  --setopt=install_weak_deps=False \
  && dnf clean all \
  && dnf autoremove -y \
  && rm -rf /usr/share/doc /usr/share/man

# Install Ansible Core via Pip
RUN pip3 install ansible-core==${ANSIBLE_VERSION}

# The imklog module is responsible for reading kernel log messages, and by commenting it out, it prevents the rsyslog service from loading that module and reduces resources needed by the container
RUN sed -i 's/^\($ModLoad imklog\)/#\1/' /etc/rsyslog.conf

# Mount necessary volumes
VOLUME ["/sys/fs/cgroup", "/tmp", "/run"]

CMD ["/lib/systemd/systemd"]
