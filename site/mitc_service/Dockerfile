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

ENV LANG=C.UTF-8
RUN gem install --source "https://rubygems.org" bundler
RUN bundle config --global frozen 1

ONBUILD RUN bundle install --jobs=4 --retry=3
