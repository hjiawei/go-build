FROM --platform=linux/amd64 calico/qemu-user-static:latest as qemu

FROM registry.access.redhat.com/ubi8/ubi:latest as ubi

COPY --from=qemu /usr/bin/qemu-*-static /usr/bin/

# Prepare a rootfs for necessary files from UBI.
RUN mkdir -p /rootfs

RUN dnf --config=/etc/dnf/dnf.conf \
    --installroot=/rootfs \
    --nodocs \
    --noplugins \
    --setopt=cachedir=/var/cache/dnf \
    --setopt=install_weak_deps=False \
    --setopt=reposdir=/etc/yum.repos.d \
    --setopt=varsdir=/etc/dnf \
    --excludepkgs=glibc-all-langpacks \
    install -y glibc

FROM scratch as source

COPY --from=ubi /rootfs /

# tmp.tar has a /tmp with the correct permissions 01777.
ADD tmp.tar /

COPY licenses /licenses/

FROM scratch

COPY --from=source / /
