FROM ruby:2.7.6 as fti

LABEL author="IdeaCrew"

ENV HOME /fti
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata libsodium-dev build-essential nodejs vim \
  && apt-get autoremove -y \
  && npm install --force --global yarn

# Configure bundler and PATH
# ENV GEM_HOME="/usr/local/bundle"

# ENV LANG=C.UTF-8 \
#   BUNDLE_PATH=$GEM_HOME \
#   BUNDLE_JOBS=4 \
#   BUNDLE_RETRY=3
# ENV BUNDLE_BIN=$BUNDLE_PATH/bin

# RUN mkdir -p $BUNDLE_PATH && mkdir -p $BUNDLE_BIN

# ENV PATH $GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

COPY . $HOME

RUN gem install bundler --version '2.2.14'
RUN cd $HOME && bundle install 
RUN cd $HOME && yarn add @rails/webpacker@5.4.0 && yarn add --dev webpack-dev-server && yarn add bootstrap && yarn add @popperjs/core
RUN cd $HOME && yarn install --check-files 


# ENV RABBITMQ_URL=amqp://${HOSTNAME}:5672
# ENV RABBITMQ_URL_EVENT_SOURCE=amqp://${HOSTNAME}
# ENV RABBITMQ_HOST=amqp://${HOSTNAME}

# RUN cd $HOME && rails webpacker:install:erb
# RUN cd $HOME && bundle install && bundle binstubs bundler
# RUN cd $HOME && yarn add @rails/webpacker@5.4.0 && yarn add --dev webpack-dev-server
# RUN cd $HOME && rails webpacker:install:erb
# RUN cd $HOME && yarn add bootstrap && yarn add @popperjs/core

# RUN unset BUNDLE_PATH
# RUN unset BUNDLE_BIN