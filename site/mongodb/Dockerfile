FROM mongo:4.2

RUN echo 'Building service: MongoDB'

# Ad and execute local .js files in alpha order
ADD . /docker-entrypoint-initdb.d

# RUN mkdir -p /data/db
# RUN mkdir -p /data/configdb
# VOLUME mongodb
# VOLUME mongodb_config
