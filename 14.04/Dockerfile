## -*- docker-image-name: "armbuild/ocs-distrib-ubuntu:trusty" -*-
FROM armbuild/ubuntu-debootstrap:trusty
MAINTAINER Online Labs <opensource@ocs.online.net>


# Environment
ENV DEBIAN_FRONTEND noninteractive
# Useful for image inheritance
ENV OCS_BASE_IMAGE armbuild/ocs-ubuntu:trusty


# Patch rootfs for docker-based builds
RUN apt-get -q update && \
    apt-get -y -qq install wget lsb-release && \
    apt-get clean && \
    wget --no-check-certificate https://codeload.github.com/online-labs/ocs-scripts/tar.gz/master -O /tmp/builder-enter.tgz && \
    tar --strip=2 -C / -xzvf /tmp/builder-enter.tgz ocs-scripts-master/skeleton-docker-based/usr/local/sbin/builder-enter && \
    rm -f /tmp/builder-enter.tgz && \
    /usr/local/sbin/builder-enter


# Install packages
ADD ./patches/etc/apt/sources.list /etc/apt/sources.list
RUN apt-get -q update && \
    apt-get -y -qq upgrade && \
    apt-get install -y -qq \
	bash-completion \
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
	nbd-client \
	net-tools \
	netcat \
	ntp \
	ntpdate \
	python-apt \
	python-yaml \
	rsync \
	rsyslog \
	screen \
	socat \
	ssh \
	sudo \
	sysstat \
	tcpdump \
	tmux \
	vim \
	wget \
	whiptail \
	xnbd-client \
    && apt-get clean


# Patch rootfs
# - Add ocs-scripts
# - Tweaks rootfs so it matches Online Labs infrastructure
RUN rm -f /etc/update-motd.d/10-help-text /etc/update-motd.d/00-header
RUN wget -qO - https://raw.githubusercontent.com/online-labs/ocs-scripts/master/upgrade_root.bash | FLAVORS=docker-based,upstart bash
ADD ./patches/etc/ /etc/


# Configure locales
RUN locale-gen en_US.UTF-8 && \
    locale-gen fr_FR.UTF-8 && \
    dpkg-reconfigure locales


# Clean rootfs from image-builder
RUN /usr/local/sbin/builder-leave