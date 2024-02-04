# syntax=docker/dockerfile:1
FROM ruby:3.3-slim-bookworm

ENV DEBIAN_FRONTEND noninteractive

# Define versions via environment variables
ENV BUNDLER_VERSION 2.5.5
ENV PG_MAJOR_VERSION 16
ENV NODE_MAJOR_VERSION 18

# Install dependencies necessary to build native gems and install other packages
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        lsb-release \
        curl \
	    gpg \
	&& rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/archives/*.deb \
    && rm -rf /tmp/*

# Install Bundler
RUN gem install bundler --version "$BUNDLER_VERSION" \
	&& bundle config --global path "$GEM_HOME" \
	&& bundle config --global bin "$GEM_HOME/bin" \
    && rm -rf /usr/local/bundle/cache/*

# Install Node.js
RUN curl -sL https://deb.nodesource.com/setup_$NODE_MAJOR_VERSION.x | bash - \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
	    nodejs \
    && corepack enable \
	&& rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/archives/*.deb \
    && rm -rf /tmp/*

# Install PostgreSQL client
RUN curl -sS https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor > /usr/share/keyrings/postgresql.gpg \
    && sh -c 'echo "deb [signed-by=/usr/share/keyrings/postgresql.gpg] http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/postgresql.list' \
    && apt-get update \
    && apt-get install -y --no-install-recommends \
	    postgresql-client-$PG_MAJOR_VERSION \
	&& rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/archives/*.deb \
    && rm -rf /tmp/*
