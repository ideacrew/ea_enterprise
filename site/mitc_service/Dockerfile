FROM ruby:2.3-stretch

LABEL author="IdeaCrew"

ENV APP_HOME /mitc
RUN mkdir -p $APP_HOME
WORKDIR $APP_HOME

RUN echo "deb http://archive.debian.org/debian stretch main" > /etc/apt/sources.list

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs vim \
  && apt-get autoremove -y \
  && npm install --force --global yarn

COPY . $APP_HOME

# Configure bundler and PATH
ENV GEM_HOME="/usr/local/bundle/2.3"
ENV LANG=C.UTF-8 \
  BUNDLE_PATH=$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

RUN gem install bundler -v 1.17.3

RUN bundle config --global frozen 1

RUN cd $APP_HOME && bundle install
RUN cd $APP_HOME && bundle pristine
