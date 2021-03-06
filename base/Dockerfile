FROM arm64v8/ubuntu:18.04

# Upgrade
RUN apt-get update -y && apt-get upgrade -y

# Install dependencies
RUN DEBIAN_FRONTEND=noninteractive apt-get -qqy --no-install-recommends install \
    bzip2 \
    ca-certificates \
    ca-certificates-java \
    tzdata \
    sudo \
    unzip \
    wget
RUN /var/lib/dpkg/info/ca-certificates-java.postinst configure

RUN DEBIAN_FRONTEND=noninteractive apt-get install apt-utils --assume-yes

RUN DEBIAN_FRONTEND=noninteractive apt install default-jre --assume-yes

RUN DEBIAN_FRONTEND=noninteractive apt update --assume-yes

RUN DEBIAN_FRONTEND=noninteractive apt-get install openjdk-8-jdk -y

#RUN rm -rf /var/lib/apt/lists/* /var/cache/apt/* \
#  && sed -i 's/securerandom\.source=file:\/dev\/random/securerandom\.source=file:\/dev\/urandom/' ./usr/lib/jvm/java-11-openjdk-arm/jre/lib/security/java.security

#===================
# Timezone settings
# Possible alternative: https://github.com/docker/docker/issues/3359#issuecomment-32150214
#===================
ENV TZ "UTC"
RUN echo "${TZ}" > /etc/timezone \
  && dpkg-reconfigure --frontend noninteractive tzdata

#==========
# Selenium
#==========
ARG SELENIUM_VERSION=3.141
ARG SELENIUM_VERSION_FULL=3.141.0

RUN mkdir -p /opt/selenium \
  && wget --no-verbose http://selenium-release.storage.googleapis.com/$SELENIUM_VERSION/selenium-server-standalone-$SELENIUM_VERSION_FULL.jar -O /opt/selenium/selenium-server-standalone.jar \
  && chmod 744 /opt/selenium/selenium-server-standalone.jar

#========================================
# Add normal user with passwordless sudo
#========================================
RUN sudo useradd seluser --shell /bin/bash --create-home \
  && sudo usermod -a -G sudo seluser \
  && echo 'ALL ALL = (ALL) NOPASSWD: ALL' >> /etc/sudoers \
  && echo 'seluser:secret' | chpasswd \
	&& chown -R seluser /opt/selenium
