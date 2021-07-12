FROM ruby:2.7.2

LABEL author="IdeaCrew"

ENV HOME /fdsh_gateway
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs vim \
  && apt-get autoremove -y \
  && npm install --force --global yarn

COPY . $HOME

# Configure bundler and PATH
ENV GEM_HOME=/web/rubygems/2.7.2
ENV LANG=C.UTF-8 \
  BUNDLE_PATH=$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=/web/rubygems/2.7.2/bin:$PATH

RUN gem install --source "https://rubygems.org" bundler
RUN bundle config --global frozen 1

ONBUILD RUN bundle install --jobs=4 --retry=3
