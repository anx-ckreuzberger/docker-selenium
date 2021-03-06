FROM selenium/node-base:2.53.0
MAINTAINER Selenium <selenium-developers@googlegroups.com>

USER root

# docker build -t selenium/node-googlechrome-500:latest --build-arg CHROME_VERSION=50.0 NodeChrome
# docker build -t selenium/node-googlechrome-490:latest --build-arg CHROME_VERSION=49.0 NodeChrome
# docker build -t selenium/node-googlechrome-480:latest --build-arg CHROME_VERSION=48.0 NodeChrome
# docker build -t selenium/node-googlechrome-470:latest --build-arg CHROME_VERSION=47.0 NodeChrome
# docker build -t selenium/node-googlechrome-460:latest --build-arg CHROME_VERSION=46.0 NodeChrome
# docker build -t selenium/node-googlechrome-450:latest --build-arg CHROME_VERSION=45.0 NodeChrome
# docker build -t selenium/node-googlechrome-440:latest --build-arg CHROME_VERSION=44.0 NodeChrome

#===============
# Google Chrome
#===============
ENV CHROME_VERSION Latest

RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list \
  && apt-get update -qqy \
  && apt-get -qqy install \
    google-chrome-stable \
  && rm /etc/apt/sources.list.d/google-chrome.list \
  && rm -rf /var/lib/apt/lists/*

#==================
# Chrome webdriver
#==================
ENV CHROME_DRIVER_VERSION 2.21
RUN wget --no-verbose -O /tmp/chromedriver_linux64.zip https://chromedriver.storage.googleapis.com/$CHROME_DRIVER_VERSION/chromedriver_linux64.zip \
  && rm -rf /opt/selenium/chromedriver \
  && unzip /tmp/chromedriver_linux64.zip -d /opt/selenium \
  && rm /tmp/chromedriver_linux64.zip \
  && mv /opt/selenium/chromedriver /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION \
  && chmod 755 /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION \
  && ln -fs /opt/selenium/chromedriver-$CHROME_DRIVER_VERSION /usr/bin/chromedriver

#========================
# Selenium Configuration
#========================
COPY generate_config /opt/selenium/generate_config
RUN /opt/selenium/generate_config > /opt/selenium/config.json

#=================================
# Chrome Launch Script Modication
#=================================
COPY chrome_launcher.sh /opt/google/chrome/google-chrome
RUN chmod +x /opt/google/chrome/google-chrome

USER seluser
# Following line fixes
# https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null
