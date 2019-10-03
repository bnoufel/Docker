FROM debian

ARG USER

RUN apt-get update

RUN apt-get install -y \
		gcc \
		g++ \
		vim \
		emacs \
		git-core \
		libc6 \
		openssl \
		make \
		sudo \
		openssh-server

		RUN adduser --disabled-password ${USER} && adduser ${USER} sudo

		RUN echo "${USER} ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

		USER ${USER}

		RUN mkdir -p /home/${USER}/work

		WORKDIR /home/${USER}/work

# To run it
# docker build --build-arg USER=user
# docker run -ti [ID]
