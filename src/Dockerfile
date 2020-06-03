FROM alpine:latest

LABEL maintainer="nico.meisenzahl@whiteduck.de" \
org.label-schema.vcs-url="https://github.com/whiteducksoftware/az-dns-forwarder"

ADD named.conf /etc/bind/

RUN apk add --no-cache bind && \
    rm -rf /var/cache/apk/*

EXPOSE 53/udp

ENTRYPOINT ["/usr/sbin/named", "-f", "-g"]
