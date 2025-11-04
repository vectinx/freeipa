# Dockerfile
ARG DOCKER_BUILD_IMAGE=registry.fedoraproject.org/fedora-toolbox:43
FROM ${DOCKER_BUILD_IMAGE}


# Workaround unreadable /etc/shadow
RUN dnf reinstall -y shadow-utils; chmod a+r /etc/shadow

COPY freeipa.spec.in freeipa.spec

# Prepare build environment
RUN rm -rf /var/cache/dnf/* && \
    dnf makecache &&\
    sudo dnf install -y \
        'dnf-command(builddep)' \
        gdb-minimal \
        make \
        autoconf \
        rpm-build \
        gettext-devel \
        git \
        automake \
        libtool \
        podman \
        python3-paramiko \
        python3-pyyaml \
        nodejs

RUN dnf builddep -y freeipa && \
    dnf builddep -y -D "with_wheels 1" -D "with_lint 1" -D "with_doc 1" \
        --best --allowerasing --setopt=install_weak_deps=False freeipa.spec && \
    rm -f freeipa.spec
