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

COPY . $HOME

RUN gem install bundler
RUN gem update --system 


RUN cd $HOME && yarn install
RUN cd $HOME && bundle install
