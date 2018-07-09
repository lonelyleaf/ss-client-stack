FROM ubuntu:18.10

EXPOSE 1080

RUN apt-get update -y
RUN apt-get install -y python-pip git &&\
    pip install -U git+https://github.com/shadowsocks/shadowsocks.git@master
ENTRYPOINT ["sslocal"]