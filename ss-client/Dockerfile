FROM ubuntu:18.04

EXPOSE 1080

RUN apt-get update -y
RUN apt-get install -y python-pip &&\
    pip install shadowsocks
ENTRYPOINT ["sslocal"]