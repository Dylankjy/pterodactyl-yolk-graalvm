FROM --platform=$TARGETOS/$TARGETARCH registry.access.redhat.com/ubi9/ubi-minimal:latest

LABEL		author="Dylan Kok" maintainer="me@dylankjy.com"
LABEL		org.opencontainers.image.source="https://github.com/Dylankjy/pterodactyl-yolk-graalvm"
LABEL		org.opencontainers.image.licenses=MIT

ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# Install basic dependencies
RUN microdnf install fontconfig glibc-langpack-en iproute openssl openssh sqlite tar wget gzip git libstdc++ -y \
    && microdnf update -y \
    && microdnf clean all -y \
    && sed -i 's/^LANG=.*/LANG="en_US.utf8"/' /etc/locale.conf

# Install OpenJDK with OpenJ9
WORKDIR /
RUN wget https://github.com/ibmruntimes/semeru21-certified-binaries/releases/download/jdk-21.0.2%2B13_openj9-0.43.0/ibm-semeru-certified-jre_x64_linux_21.0.2.0.tar.gz \
    && mkdir java \
    && tar -xzf ibm-semeru-certified-jre_x64_linux_21.0.2.0.tar.gz -C /java \
    && rm ibm-semeru-certified-jre_x64_linux_21.0.2.0.tar.gz

ENV JAVA_HOME=/java/jdk-21.0.2+13-jre
ENV	PATH=/java/jdk-21.0.2+13-jre/bin:$PATH

# Add container user
RUN adduser --home-dir /home/container -u 998 container
USER		container
ENV			USER=container HOME=/home/container
WORKDIR		/home/container

# Create .ssh directory
RUN mkdir .ssh

# Add Github SSH fingerprint
RUN ssh-keyscan github.com >> ~/.ssh/known_hosts

COPY		./entrypoint.sh /entrypoint.sh
CMD			[ "/bin/bash", "/entrypoint.sh" ]