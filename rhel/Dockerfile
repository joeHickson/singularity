FROM redhat/ubi8:latest

# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:/usr/src/cylc/bin:/usr/src/rose/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8
# https://github.com/docker-library/python/issues/147
ENV PYTHONIOENCODING UTF-8

#install all the system packages
RUN dnf install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-8.noarch.rpm \
	&& dnf -y --setopt=install_weak_deps=False --setopt=tsflags=nodocs install \
      http://mirror.centos.org/centos/8-stream/BaseOS/x86_64/os/Packages/centos-gpg-keys-8-6.el8.noarch.rpm \
      http://mirror.centos.org/centos/8-stream/BaseOS/x86_64/os/Packages/centos-stream-repos-8-6.el8.noarch.rpm \
 	&& dnf -y --setopt=install_weak_deps=False --setopt=tsflags=nodocs install epel-release \
 	&& dnf config-manager --set-enabled powertools \
	&& yum install -y \
		graphviz \
		sqlite \
		python2-pip \
		python2-jinja2  \
		gcc \
		make \
		wget \
		pygtk2 \
		graphviz-devel \
		python2-devel \
		python2-requests \
	&& pip2 install \
		pygraphviz \
		"pyOpenSSL<19.1" \
	&& dnf remove -y \
		gcc
# install rose and cylc
RUN wget -O cylc.tar.gz "https://github.com/cylc/cylc-flow/archive/refs/tags/7.9.8.tar.gz" \
	&& mkdir -p /usr/src/cylc \
	&& tar -xzC /usr/src/cylc --strip-components=1 -f cylc.tar.gz \
	&& rm cylc.tar.gz \
	&& cd /usr/src/cylc \
    && make version  \
	&& cylc check-software \
	&& wget -O rose.tar.gz "https://github.com/metomi/rose/archive/refs/tags/2019.01.8.tar.gz" \
	&& mkdir -p /usr/src/rose \
	&& tar -xzC /usr/src/rose --strip-components=1 -f rose.tar.gz \
	&& rm rose.tar.gz \
	&& cd /usr/src/rose 