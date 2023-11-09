FROM ubuntu:18.04


# ensure local python is preferred over distribution python
ENV PATH /usr/local/bin:/usr/src/cylc/bin:/usr/src/rose/bin:$PATH

# http://bugs.python.org/issue19846
# > At the moment, setting "LANG=C" on a Linux system *fundamentally breaks Python 3*, and that's not OK.
ENV LANG C.UTF-8
# https://github.com/docker-library/python/issues/147
ENV PYTHONIOENCODING UTF-8

#install all the system packages
RUN apt-get update && \
	apt-get install -q -y \
		at \
		python-pip \
		graphviz \
		python-jinja2 \
		python-pygraphviz \
		python-gtk2 \
		sqlite3 \
		pep8 \
		wget \
		gfortran \
		pcregrep \
		lxterminal \
		tidy \
		python-requests \
		python-virtualenv && \
    pip install \
		mock \
		pytest-tap 

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