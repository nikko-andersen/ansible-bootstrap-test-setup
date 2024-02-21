FROM ubuntu:latest

# Add sudo and openssh-server
ENV DEBIAN_FRONTEND=noninteractive
RUN apt update && apt install openssh-server sudo -y

# start ssh with port exposed
USER root
RUN service ssh start

EXPOSE 22

RUN /usr/sbin/sshd

CMD ["/bin/bash"] 
# either use this and run in -it mode or use an option to CMD to sleep infinity