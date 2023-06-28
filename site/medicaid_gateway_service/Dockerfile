FROM ruby:2.7.6

LABEL author="IdeaCrew"

ENV HOME /medicaid_gateway
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs libsodium23 git vim default-jre \
  && apt-get autoremove -y \
  && npm install --force --global yarn

COPY . $HOME

RUN gem install bundler -v 2.3.16

RUN cd $HOME && yarn install --check-files 
RUN cd $HOME && bundle install
RUN cd $HOME && bin/rails dev:cache
