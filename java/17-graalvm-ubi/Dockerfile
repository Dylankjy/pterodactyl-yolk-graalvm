# Forked from Matthew Penner (matthewpi/images), MIT License
# Modified by Dylan Kok to run graalvm

FROM		--platform=$TARGETOS/$TARGETARCH registry.access.redhat.com/ubi8/ubi-minimal:latest

LABEL		author="Dylan Kok" maintainer="me@dylankjy.com"

LABEL		org.opencontainers.image.source="https://github.com/Dylankjy/pterodactyl-yolk-graalvm"
LABEL		org.opencontainers.image.licenses=MIT

RUN			microdnf install fontconfig glibc-langpack-en iproute openssl sqlite tar wget gzip \
				&& microdnf update \
				&& microdnf install shadow-utils \
				&& adduser --home-dir /home/container container \
				&& microdnf remove shadow-utils \
				&& microdnf clean all \
				&& sed -i 's/^LANG=.*/LANG="en_US.utf8"/' /etc/locale.conf

WORKDIR     /

RUN         wget https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-22.1.0/graalvm-ce-java17-linux-amd64-22.1.0.tar.gz \
			&& mkdir jvm \
			&& tar -xzf graalvm-ce-java17-linux-amd64-22.1.0.tar.gz -C /jvm

ENV			LANG=en_US.UTF-8
ENV			LANGUAGE=en_US:en
ENV			LC_ALL=en_US.UTF-8
ENV			JAVA_HOME=/jvm/graalvm-ce-java17-22.1.0
ENV			PATH=/jvm/graalvm-ce-java17-22.1.0/bin:$PATH

USER		container
ENV			USER=container HOME=/home/container
WORKDIR		/home/container

COPY		./entrypoint.sh /entrypoint.sh
CMD			[ "/bin/bash", "/entrypoint.sh" ]