FROM ruby:2.5.9

LABEL author="IdeaCrew"

ENV HOME /enroll
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -

RUN wget https://dl.yarnpkg.com/debian/pubkey.gpg 
RUN cat pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs git libsodium23 vim yarn=1.22.4-1 \
  && apt-get autoremove -y \
  && apt-get install libxi6 libnss3 \
  libgconf-2-4 \
  fonts-liberation \
  libappindicator3-1 \
  libasound2 \
  libatk-bridge2.0-0 \
  libatk1.0-0 \
  libcups2 \
  libgtk-3-0 \
  libx11-xcb1 \
  libxss1 \
  lsb-release \
  xdg-utils \
  libxcomposite1 -y \
  chromium chromium-driver

ENV LANG=C.UTF-8 \
  SHELL=/bin/bash 

RUN echo 'chromedriver $@ --whitelisted-ips' > /usr/local/bin/hackedchromedriver

RUN chmod +x /usr/local/bin/hackedchromedriver

RUN gem install bundler -v 2.3.16

COPY . $HOME
RUN cd $HOME && bundle install
RUN cd $HOME && yarn install --network-timeout 100000 
RUN cd $HOME && npx browserslist@latest --update-db
RUN cd $HOME/components/benefit_markets && bundle install

