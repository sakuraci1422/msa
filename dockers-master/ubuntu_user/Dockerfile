FROM ubuntu
MAINTAINER The NamjungGu <nowage@gmail.com>
COPY ./start.sh /
RUN chmod 755 /start.sh
RUN apt update -y
RUN apt install -y python3.10
RUN ln -s /usr/bin/python3.10 /usr/bin/python
# ENTRYPOINT ["/xx/sshd","sshd"]
CMD /start.sh
