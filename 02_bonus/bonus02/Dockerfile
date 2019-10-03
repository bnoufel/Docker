FROM debian:latest

ARG USER

RUN apt-get update && apt-get upgrade -y

RUN apt-get install -y \
	vim \
	curl \
	wget \
	git-core \
	sudo

RUN adduser --disabled-password ${USER} && adduser ${USER} sudo

RUN echo "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
USER ${USER}
WORKDIR /home/${USER}
RUN curl -O https://dl.google.com/go/go1.10.2.linux-amd64.tar.gz
RUN tar xvf go1.10.2.linux-amd64.tar.gz

RUN sudo chown -R root:root ./go
RUN sudo mv go /usr/local
ENV GOPATH="/home/${USER}/work"
ENV PATH=${PATH}:/usr/local/go/bin:${GOPATH}/bin
RUN echo "export GOPATH=/home/${USER}/work" >> ~/.profile
RUN echo "export PATH=${PATH}" >> ~/.profile
#CMD ["sudo", "source", "~/.profile"]

RUN mkdir -p /home/${USER}/work
WORKDIR /home/${USER}/work

# to run it
# docker build --build-arg USER=user .
#  docker run -it [ID]
