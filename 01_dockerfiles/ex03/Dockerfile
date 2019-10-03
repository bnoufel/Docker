FROM ubuntu:14.04

ARG IP_ADDRESS
ENV IP_ADDRESS=${IP_ADDRESS} TZ="Europe/Berlin" TERM=xterm

# Install required packages
RUN apt-get update -q \
	&& DEBIAN_FRONTEND=noninteractive apt-get install -yq --no-install-recommends \
	ca-certificates \
	openssh-server \
	wget \
	apt-transport-https \
	git-core \
	tzdata

# Download & Install GitLab
RUN echo "deb https://packages.gitlab.com/gitlab/gitlab-ce/ubuntu/ `lsb_release -cs` main" > /etc/apt/sources.list.d/gitlab_gitlab-ce.list
RUN wget -q -O - https://packages.gitlab.com/gpg.key | apt-key add -
RUN apt-get update && apt-get install -yq --no-install-recommends gitlab-ce

# Manage SSHD through runit
RUN mkdir -p /opt/gitlab/sv/sshd/supervise && mkfifo /opt/gitlab/sv/sshd/supervise/ok && printf "#!/bin/sh\nexec 2>&1\numask 077\nexec /usr/sbin/sshd -D" > /opt/gitlab/sv/sshd/run && chmod a+x /opt/gitlab/sv/sshd/run && ln -s /opt/gitlab/sv/sshd /opt/gitlab/service && mkdir -p /var/run/sshd

# Fit config in gitlab.rb
RUN sed -ie "s/external_url 'http:\/\/gitlab.example.com'/external_url 'https:\/\/${IP_ADDRESS}'/g" /etc/gitlab/gitlab.rb
RUN echo "gitlab_rails['time_zone'] = 'Europe/Berlin'\npostgresql['shared_buffers'] = '1MB'\ngitlab_rails['rake_cache_clear'] = false\ngitlab_rails['gitlab_shell_ssh_port'] = 23" >> /etc/gitlab/gitlab.rb
ENV EXTERNAL_URL="https://$IP_ADDRESS" GITLAB_SHELL_SSH_PORT=23

# Enable HTTPS
RUN openssl genrsa -out ${IP_ADDRESS}.key 2048
RUN openssl req -new -key ${IP_ADDRESS}.key -out ${IP_ADDRESS}.csr -subj "/C=FR/L=IDF/O=Paris/OU=42/CN=${IP_ADDRESS}/emailAddress=bnoufel@student.42.fr"
RUN openssl x509 -req -days 3650 -in ${IP_ADDRESS}.csr -signkey ${IP_ADDRESS}.key -out ${IP_ADDRESS}.pem
RUN mkdir -p /etc/gitlab/trusted-certs/ && chmod 700 /etc/gitlab/trusted-certs/ && cp ${IP_ADDRESS}.pem /etc/gitlab/trusted-certs/

# Create a wrapper to handle signal, trigger runit and reconfigure GitLab
WORKDIR /usr/local/bin
RUN wget -q https://gitlab.com/gitlab-org/omnibus-gitlab/raw/b2a397f2c407fcd22b39b85e91932dcd6163332a/docker/assets/wrapper
RUN sed -i '21,45d' /usr/local/bin/wrapper && sed -i '71,74d' /usr/local/bin/wrapper
RUN chmod +x /usr/local/bin/wrapper

# Expose web & ssh && database
EXPOSE 443 80 22 6379 5432 8080

# Define data volumes
VOLUME ["/etc/gitlab", "/var/opt/gitlab", "/var/log/gitlab"]

CMD ["/usr/local/bin/wrapper"]

#######################
###### to build #######
#######################
# docker build --build-arg IP_ADDRESS=$(docker-machine ip Char) .
#######################
##### to run it #######
#######################
#docker run -itd --restart unless-stopped -p 443:443 -p 80:80 -p 23:22 [ID]
