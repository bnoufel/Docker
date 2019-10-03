FROM debian

RUN apt-get update && apt-get upgrade -y && apt-get install wget sudo tar bzip2 -y

RUN adduser --disabled-password teamspeak && adduser teamspeak sudo\
	&& chown -R teamspeak:teamspeak /home/teamspeak

RUN wget https://files.teamspeak-services.com/releases/server/3.5.0/teamspeak3-server_linux_amd64-3.5.0.tar.bz2
RUN tar xfv teamspeak3*
RUN mv teamspeak3-server_linux_amd64/* /home/teamspeak
RUN rm -rf teamspeak3*

EXPOSE 9987/udp 10011 30033

WORKDIR	/home/teamspeak/
ENV TS3SERVER_LICENSE accept
RUN mkdir /data && chown teamspeak:teamspeak /data

VOLUME	["/data"]

USER teamspeak
CMD ["sh", "/home/teamspeak/ts3server_minimal_runscript.sh"]

#to run it
# docker run -it -p 30033:30033 -p 10011:10011 -p 9987:9987/UDP [ID] 


