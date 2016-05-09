FROM selenium/node-base:2.53.0
MAINTAINER Selenium <selenium-developers@googlegroups.com>

USER root



# build: docker build -t selenium/node-firefox-4502:latest --build-arg FIREFOX_VERSION=45.0.2 NodeFirefox
# build: docker build -t selenium/node-firefox-4601:latest --build-arg FIREFOX_VERSION=46.0.1 NodeFirefox
# build: docker build -t selenium/node-firefox-4402:latest --build-arg FIREFOX_VERSION=44.0.2 NodeFirefox


#=========
# Firefox
#=========
ARG FIREFOX_VERSION
ENV FIREFOX_VERSION ${FIREFOX_VERSION:-45.0.2}

RUN apt-get update -qqy \
  && apt-get -qqy --no-install-recommends install firefox \
  && rm -rf /var/lib/apt/lists/* \
  && wget --no-verbose -O /tmp/firefox.tar.bz2 https://download-installer.cdn.mozilla.net/pub/firefox/releases/$FIREFOX_VERSION/linux-x86_64/en-US/firefox-$FIREFOX_VERSION.tar.bz2 \
  && apt-get -y purge firefox \
  && rm -rf /opt/firefox \
  && tar -C /opt -xjf /tmp/firefox.tar.bz2 \
  && rm /tmp/firefox.tar.bz2 \
  && mv /opt/firefox /opt/firefox-$FIREFOX_VERSION \
  && ln -fs /opt/firefox-$FIREFOX_VERSION/firefox /usr/bin/firefox

#========================
# Selenium Configuration
#========================
COPY generate_config /opt/selenium/generate_config
RUN /opt/selenium/generate_config > /opt/selenium/config.json

USER seluser
