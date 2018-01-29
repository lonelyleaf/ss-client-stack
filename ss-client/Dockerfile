FROM ubuntu:18.04

EXPOSE 1080

RUN apt-get install python-pip &&\
    pip install shadowsocks
ENTRYPOINT ["sslocal"]