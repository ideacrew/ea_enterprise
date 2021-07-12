FROM ruby:2.3

LABEL author="IdeaCrew"

ENV APP_HOME /mitc
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs vim \
  && apt-get autoremove -y \
  && npm install --force --global yarn

COPY . $APP_HOME
# ADD Gemfile* $APP_HOME/

# Configure bundler and PATH
ENV GEM_HOME="/usr/local/bundle/2.3"
ENV LANG=C.UTF-8 \
  BUNDLE_PATH=$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

RUN gem install --source "https://rubygems.org" bundler
RUN bundle config --global frozen 1

ONBUILD RUN yarn install
ONBUILD RUN bundle install
