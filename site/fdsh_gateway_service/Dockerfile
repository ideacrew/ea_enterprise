FROM ruby:2.7.3

LABEL author="IdeaCrew"

ENV HOME /fdsh_gateway
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -

RUN wget https://dl.yarnpkg.com/debian/pubkey.gpg 
RUN cat pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs git libsodium23 vim yarn=1.22.4-1 \
  && apt-get autoremove -y 
  # && apt-get install libxi6 libnss3 \
  # libgconf-2-4 \
  # fonts-liberation \
  # libappindicator3-1 \
  # libasound2 \
  # libatk-bridge2.0-0 \
  # libatk1.0-0 \
  # libcups2 \
  # libgtk-3-0 \
  # libx11-xcb1 \
  # libxss1 \
  # lsb-release \
  # xdg-utils \
  # libxcomposite1 -y \
  # chromium chromium-driver

COPY . $HOME

# Configure bundler and PATH
# ENV GEM_HOME="/usr/local/bundle/2.7.3"
# ENV LANG=C.UTF-8 \
#   BUNDLE_PATH=$GEM_HOME \
#   BUNDLE_JOBS=4 \
#   BUNDLE_RETRY=3 \
#   PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

# RUN gem install --source "https://rubygems.org" bundler
# RUN bundle config --global frozen 1

RUN gem install bundler
RUN gem update --system 


RUN cd $HOME && yarn install
RUN cd $HOME && bundle install
