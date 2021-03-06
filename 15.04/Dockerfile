## -*- docker-image-name: "armbuild/scw-distrib-ubuntu:vivid" -*-
FROM armbuild/ubuntu-debootstrap:vivid
MAINTAINER Scaleway <opensource@scaleway.com> (@scaleway)


# Environment
ENV DEBIAN_FRONTEND noninteractive
# Useful for image inheritance
ENV SCW_BASE_IMAGE armbuild/scw-ubuntu:vivid


# Patch rootfs for docker-based builds
RUN apt-get -y -q update \
 && apt-get -y -qq install bash curl lsb-release tar \
 && apt-get -y clean \
 && curl -Lq http://j.mp/scw-skeleton | FLAVORS=common,docker-based,systemd bash -e \
 && /usr/local/sbin/builder-enter


ADD ./patches/etc/apt/sources.list /etc/apt/sources.list
# Install packages
RUN apt-get -q update && \
    apt-get -y --force-yes -qq upgrade && \
    apt-get -y --force-yes -qq install \
	bash-completion \
	bc \
	ca-certificates \
	cron \
	curl \
	dbus \
	dstat \
	ethstatus \
	file \
	fio \
	htop \
	ioping \
	iotop \
	iperf \
	iptables \
	iputils-ping \
	isc-dhcp-client \
	less \
	locate \
	lsb-release \
	lsof \
	make \
	man-db \
	mg \
	mosh \
	nano \
	net-tools \
	netcat \
	ntp \
	ntpdate \
	python-apt \
	python-yaml \
	rsync \
	rsyslog \
	screen \
	shunit2 \
	socat \
	ssh \
	sudo \
	sysstat \
	systemd-sysv \
	tcpdump \
	tmux \
	vim \
	wget \
	whiptail \
    && apt-get clean


# Patch rootfs
# - Tweaks rootfs so it matches Scaleway infrastructure
RUN rm -f /etc/update-motd.d/10-help-text /etc/update-motd.d/00-header
ADD ./patches/etc/ /etc/


# Configure locales
RUN locale-gen en_US.UTF-8 && \
	locale-gen fr_FR.UTF-8 && \
	dpkg-reconfigure locales




# Default target
RUN systemctl set-default multi-user


# Configure Systemd
RUN systemctl enable \
        oc-fetch-ssh-keys \
	oc-sync-kernel-modules \
	oc-generate-ssh-keys \
	oc-add-extra-volumes \
	oc-gen-machine-id \
 && systemctl disable \
	systemd-modules-load.service \
	dev-ttyS0.device \
	systemd-update-utmp-runlevel \
	proc-sys-fs-binfmt_misc.automount \
	kmod-static-nodes.service


RUN rm -f /sbin/init && cp /lib/systemd/systemd /sbin/init


# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave
