#
#  This dockerfile builds a new centos container from absolutely scratch
#  to minimize the attack surface of vulnerable packages
#

FROM    centos:7.6.1810 as builder


# destination folder of the new os
ARG     centos_root=/centos_image/rootfs


# make sure everything is the latest
RUN     set -eux; yum -y update


# starting to build up my os from scratch
RUN     set -eux; \
        mkdir -p ${centos_root} && \
        rpm --root ${centos_root} --initdb && \
        yum reinstall --downloadonly --downloaddir . centos-release && \
        rpm --root ${centos_root} -ivh centos-release*.rpm && \
        rpm --root ${centos_root} --import  ${centos_root}/etc/pki/rpm-gpg/RPM-GPG-KEY-CentOS-7


# installing mandatory packages
RUN     set -eux; yum -y --installroot=${centos_root} \
        --setopt=tsflags='nodocs' \
        --setopt=override_install_langs=en_US.utf8 \
        install \
        glibc \
        libtool-ltdl \
        ca-certificates \
        shadow-utils \
        tar \
        gzip \
        findutils \
        python \
        python-pip


# How do I reduce the size of locale-archive?
RUN     set -eux; \
        rm -rf ${centos_root}/var/cache/yum && \
        echo "localedef --list-archive | grep -v -i ^en | xargs localedef --delete-from-archive" > ${centos_root}/fixme.sh && \
        echo "mv /usr/lib/locale/locale-archive /usr/lib/locale/locale-archive.tmpl" >> ${centos_root}/fixme.sh && \
        echo "build-locale-archive" >> ${centos_root}/fixme.sh && \
        chmod 755 ${centos_root}/fixme.sh && \
        chroot ${centos_root} /fixme.sh && \
        rm ${centos_root}/fixme.sh


#
#  FINAL CONTAINER
#

FROM    scratch

COPY    --from=builder /centos_image/rootfs /
