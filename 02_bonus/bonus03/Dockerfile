FROM debian:latest
ARG USER
RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y \
	vim \
	curl \
	wget \
	git-core \
	gcc \
	sudo
RUN adduser --disabled-password ${USER} && adduser ${USER} sudo

RUN echo "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER ${USER}
ENV HOME=/home/${USER}
WORKDIR ${HOME}

#RUN curl -f -L https://static.rust-lang.org/rustup.sh -O
#RUN sh rustup.sh
RUN curl https://sh.rustup.rs --output rustup.sh
RUN chmod +x rustup.sh
RUN ./rustup.sh -y
ENV PATH=${PATH}:${HOME}/.cargo/bin
RUN echo "PATH=${PATH}:${HOME}/.cargo/bin" > ~/.profile

RUN mkdir -p ${HOME}/work
WORKDIR ${HOME}/work

# to run it
#  docker build --build-arg USER=test .
# docker run -ti [ID]
