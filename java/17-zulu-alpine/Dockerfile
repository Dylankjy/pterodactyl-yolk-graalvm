FROM alpine:latest

LABEL		author="Dylan Kok" maintainer="me@dylankjy.com"
LABEL		org.opencontainers.image.source="https://github.com/Dylankjy/pterodactyl-yolk-graalvm"
LABEL		org.opencontainers.image.licenses=MIT

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install basic dependencies
RUN apk add fontconfig gcompat sqlite iproute2 openssl tar wget gzip git openssh-client

# Install Azul Zulu
RUN wget --quiet https://cdn.azul.com/public_keys/alpine-signing@azul.com-5d5dc44c.rsa.pub -P /etc/apk/keys/ && \
    apk --repository https://repos.azul.com/zulu/alpine --no-cache add zulu17-jre~=17.0.3 tzdata

ENV JAVA_HOME=/usr/lib/jvm/zulu17-ca
ENV	PATH=/usr/lib/jvm/zulu17-ca/bin:$PATH

# Add container user
RUN adduser -D -h /home/container container
USER		container
ENV			USER=container HOME=/home/container
WORKDIR		/home/container

COPY		./entrypoint.sh /entrypoint.sh
CMD			[ "/bin/ash", "/entrypoint.sh" ]