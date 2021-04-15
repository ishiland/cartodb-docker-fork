FROM ubuntu:18.04

RUN apt-get update && apt-get install -y -q \
    curl \
    git-core \
    git

RUN curl -sL https://deb.nodesource.com/setup_10.x -o nodesource_setup.sh &&  \
    bash nodesource_setup.sh && \
    apt install -y -q nodejs

RUN  npm install -g grunt-cli && \
  npm install -g npm@6


# CartoDB/CartoDB-SQL-API version
ENV CARTODB_SQL_API_VERSION=master

RUN git clone git://github.com/CartoDB/CartoDB-SQL-API.git && \
    cd CartoDB-SQL-API && \
    git checkout $SQLAPI_VERSION && \
    npm install && \
    mkdir logs

# Copy resources.
COPY config/*.js /CartoDB-SQL-API/config/environments/
COPY docker-entrypoint.sh /usr/local/bin/

WORKDIR /CartoDB-SQL-API

EXPOSE 80

# We configure the application using just the base config here. Additional configuration files can
# be merged with the base by supplying their filenames as the CMD.
ENTRYPOINT [ "docker-entrypoint.sh", "base.config.js" ]

# We use the development example config by default. Alternatives:
# - production.js.example
# - staging.js.example
# - test.js.example
# OR extend this image by COPYing a custom config file into /CartoDB-SQL-API/config/environments/
# and passing its name as the CMD.
CMD [ "production.js.example" ]
