FROM alpine:3.7

EXPOSE 2080

RUN apk --no-cache --update add privoxy bash
RUN chown privoxy.privoxy /etc/privoxy/*
#COPY ./config /etc/privoxy/config
ENTRYPOINT ["privoxy"]
CMD ["--no-daemon","--user","privoxy","/etc/privoxy/privoxy.config"]