FROM ruby:2.5.9

LABEL author="IdeaCrew"

ENV HOME /enroll
RUN mkdir -p $HOME
WORKDIR $HOME

RUN curl -sL https://deb.nodesource.com/setup_14.x | bash -

RUN wget https://dl.yarnpkg.com/debian/pubkey.gpg 
RUN cat pubkey.gpg | apt-key add -
RUN echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list

RUN apt-get update \
  && apt-get -yq dist-upgrade \
  && apt-get install -y tzdata build-essential nodejs git libsodium23 vim yarn=1.22.4-1 \
  && apt-get autoremove -y \
  && apt-get install libxi6 libnss3 \
  libgconf-2-4 \
  fonts-liberation \
  libappindicator3-1 \
  libasound2 \
  libatk-bridge2.0-0 \
  libatk1.0-0 \
  libcups2 \
  libgtk-3-0 \
  libx11-xcb1 \
  libxss1 \
  lsb-release \
  xdg-utils \
  libxcomposite1 -y \
  chromium chromium-driver

# ARG USER_ID
# ARG GROUP_ID

# RUN addgroup --gid $GROUP_ID user
# RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user
# RUN chown -R user:user $HOME

# RUN curl -L -o google-chrome.deb https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
# RUN dpkg -i google-chrome.deb
# RUN sed -i 's|HERE/chrome\"|HERE/chrome\" --disable-setuid-sandbox|g' /opt/google/chrome/google-chrome
# RUN rm google-chrome.deb

# COPY . $HOME

# Configure bundler and PATH
# ENV GEM_HOME="/usr/local/bundle/2.5"
ENV LANG=C.UTF-8 \
  # BUNDLE_PATH=$GEM_HOME \
  # BUNDLE_JOBS=4 \
  # BUNDLE_RETRY=3 \
  SHELL=/bin/bash 
  # PATH=$GEM_HOME/bin:$GEM_HOME/gems/bin:$PATH

RUN echo 'chromedriver $@ --whitelisted-ips' > /usr/local/bin/hackedchromedriver

RUN chmod +x /usr/local/bin/hackedchromedriver

# ARG BUNDLER_VERSION_OVERRIDE
# ENV BUNDLER_VERSION=$BUNDLER_VERSION_OVERRIDE
# RUN gem update --system && gem install bundler:$BUNDLER_VERSION
# RUN bundle config --global frozen 1

RUN gem install bundler -v 2.3.16
RUN gem update --system 

COPY . $HOME
# RUN sed -i '/webdrivers/d' Gemfile


# RUN ls -ltr /enroll/public/
# RUN ls -ltr /enroll/node_modules/
#&& yarn install --check-files
RUN cd $HOME && bundle install
RUN cd $HOME && yarn install --network-timeout 100000 

