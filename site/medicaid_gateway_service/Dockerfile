FROM ruby:2.7.2

LABEL author="IdeaCrew"

ENV HOME /medicaid_gateway
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs vim default-jre \
  && apt-get autoremove -y \
  && npm install --force --global yarn

COPY . $HOME

# Configure bundler and PATH
ENV GEM_HOME="/usr/local/bundle/2.7.2"
ENV LANG=C.UTF-8 \
  BUNDLE_PATH=$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

RUN gem install --source "https://rubygems.org" bundler
RUN bundle config --global frozen 1

RUN cd $HOME && yarn install
RUN cd $HOME && bundle install
