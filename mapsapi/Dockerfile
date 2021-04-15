FROM ubuntu:18.04

RUN apt-get update \
  && apt-get -y -q install \
    build-essential \
    git \
    curl \
    libcairo2-dev \
    libgif-dev \
    libjpeg-dev \
    libpango1.0-dev \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sL https://deb.nodesource.com/setup_12.x -o nodesource_setup.sh &&  \
    bash nodesource_setup.sh && \
    apt install -y -q nodejs

# CartoDB/Windshaft-cartodb version
ENV WINDSHAFT_CARTODB_VERSION=master

# Install the application.
RUN git clone git://github.com/CartoDB/Windshaft-cartodb.git && \
    cd Windshaft-cartodb && \
    git checkout $WINDSHAFT_VERSION && \
    npm install && \
    mkdir logs

# Copy resources.
COPY config/*.js /Windshaft-cartodb/config/environments/
COPY docker-entrypoint.sh /usr/local/bin/

WORKDIR /Windshaft-cartodb

EXPOSE 80

# We configure the application using just the base config here. Additional configuration files can
# be merged with the base by supplying their filenames as the CMD.
ENTRYPOINT [ "docker-entrypoint.sh", "base.config.js" ]

# We use the development example config by default. Alternatives:
# - production.js.example
# - staging.js.example
# - test.js.example
# OR extend this image by COPYing a custom config file into /Windshaft-cartodb/config/environments/
# and passing its name as the CMD.
CMD [ "production.js.example" ]
