FROM ruby:2.7.3

LABEL author="IdeaCrew"

ENV HOME /polypress
RUN mkdir -p $HOME

# ENV YARN_CACHE_FOLDER /yarn
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential libsodium23 git nodejs vim xfonts-base xfonts-75dpi \
  && apt-get autoremove -y \
  && npm install --force --global yarn

RUN wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_arm64.deb \
  && dpkg -i wkhtmltox_0.12.6-1.buster_arm64.deb \
  && rm wkhtmltox_0.12.6-1.buster_arm64.deb

# ARG USER_ID
# ARG GROUP_ID

# RUN addgroup --gid $GROUP_ID user
# RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user
# RUN chown -R user:user $HOME

COPY . $HOME

# Configure bundler and PATH
ENV GEM_HOME="/usr/local/bundle/2.7"
ENV LANG=C.UTF-8 \
  BUNDLE_PATH=$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

# RUN gem install --source "https://rubygems.org" bundler
RUN gem install bundler -v 2.4.22
RUN bundle config --global frozen 1

RUN cd $HOME && bundle install
RUN cd $HOME && yarn install --check-files
