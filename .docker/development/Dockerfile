FROM ruby:2.7.2

LABEL author="IdeaCrew"

ENV HOME /ea_enterprise
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs vim \
  && apt-get autoremove -y \
  && npm install --global yarn

COPY . $HOME

# Setting env up
# ENV RAILS_ENV='development'
# ENV NODE_ENV='development'

# Configure bundler and PATH
ENV LANG=C.UTF-8 \
  GEM_HOME=/web/rubygems/2.7.2 \
  BUNDLE_PATH=/$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=/web/rubygems/2.7.2/bin:$PATH

# RUN gem install bundler
# RUN gem install --source "https://rubygems.org" # bundler:2.2.14

RUN mkdir -p /web/service
WORKDIR /web/service

RUN bundle config --global frozen 1

ONBUILD COPY Gemfile Gemfile
ONBUILD COPY Gemfile.lock Gemfile.lock

ONBUILD RUN bundle install --jobs=4 --retry=3
ONBUILD COPY ./ /web/service/
