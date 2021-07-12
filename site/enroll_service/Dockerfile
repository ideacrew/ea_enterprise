FROM ruby:2.5.1

LABEL author="IdeaCrew"

ENV HOME /enroll
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_12.x | bash -

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs vim \
  && apt-get autoremove -y \
  && npm install --force --global yarn

# ARG USER_ID
# ARG GROUP_ID

# RUN addgroup --gid $GROUP_ID user
# RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user
# RUN chown -R user:user $HOME

COPY . $HOME

# Configure bundler and PATH
ENV GEM_HOME="/usr/local/bundle/2.5"
ENV LANG=C.UTF-8 \
  BUNDLE_PATH=$GEM_HOME \
  BUNDLE_JOBS=4 \
  BUNDLE_RETRY=3 \
  PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

RUN gem install --source "https://rubygems.org" bundler
RUN bundle config --global frozen 1

ONBUILD RUN yarn install
ONBUILD RUN bundle install

# Add a script to be executed every time the container starts.
COPY ./entrypoint.sh /usr/bin
RUN chmod +x /usr/bin/entrypoint.sh
ENTRYPOINT ["entrypoint.sh"]
