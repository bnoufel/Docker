FROM debian
ARG RUBY_VERSION
ARG USER
RUN apt-get update && \
	apt-get install -y \
	git-core \
	vim \
	wget \
	curl \
	procps \
	gnupg2 \
	bzip2 \
	gawk \
	g++ \
	gcc \
	sudo \
	autoconf \
	automake \
	bison \
	libc6-dev \
	libffi-dev \
	libgdbm-dev \
	libncurses5-dev \
	libsqlite3-dev \
	libtool \
	libyaml-dev \
	make \
	pkg-config \
	sqlite3 \
	zlib1g-dev \
	libgmp-dev \
	libreadline-dev \
	libssl-dev

RUN echo "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN adduser --disabled-password ${USER} && adduser ${USER} sudo
RUN echo "gpg --no-tty --keyserver hkp://keys.gnupg.net --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB" > key.sh
RUN chmod +x key.sh && ./key.sh
RUN \curl -L https://get.rvm.io | bash -s stable
RUN /bin/bash -l -c "rvm requirements"
RUN /bin/bash -l -c "rvm install ${RUBY_VERSION}"
RUN /bin/bash -l -c "gem install bundler --no-ri --no-rdoc"
USER ${USER}
RUN mkdir -p /home/${USER}/work
WORKDIR /home/${USER}/work
ENTRYPOINT ["/bin/bash", "-l", "-c"]

# to run it
# docker build --build-arg RUBY_VERSION=x.x.x --build-arg USER=user .
# docker run -ti [ID]
