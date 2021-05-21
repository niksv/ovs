FROM ubuntu:20.04
MAINTAINER "Aliasgar Ginwala" <aginwala@ebay.com>

ENV TZ=Europe/Saratov
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ARG OVS_BRANCH
ARG KERNEL_VERSION
ARG GITHUB_SRC
ARG DISTRO

RUN apt-get update
RUN apt-get install -y linux-image-$KERNEL_VERSION linux-headers-$KERNEL_VERSION
RUN apt-get install -y apt-utils libelf-dev build-essential libssl-dev python3 wget gdb autoconf libtool git automake bzip2 debhelper dh-autoreconf openssl
RUN apt-get install -y vim kmod net-tools uuid-runtime iproute2
RUN apt-get install -y openssh-server
RUN yes password | passwd root
RUN mkdir -p ~/.ssh && chmod 700 ~/.ssh && echo ${ID_RSA} >>  ~/.ssh/authorized_keys
# RUN useradd -m user && yes password | passwd user

COPY ovs.tar.gz ovs.tar.gz
copy $DISTRO/build-kernel-modules.sh /build-kernel-modules.sh
RUN /build-kernel-modules.sh $KERNEL_VERSION $OVS_BRANCH $GITHUB_SRC

COPY create_ovs_db.sh /etc/openvswitch/create_ovs_db.sh
RUN /etc/openvswitch/create_ovs_db.sh

COPY ovs-override.conf /etc/depmod.d/openvswitch.conf

COPY start-ovs /bin/start-ovs
VOLUME ["/var/log/openvswitch", "/var/lib/openvswitch",\
 "/var/run/openvswitch", "/etc/openvswitch"]
ENTRYPOINT ["start-ovs"]
