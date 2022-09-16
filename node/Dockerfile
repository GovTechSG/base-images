ARG NODE_MAJOR_VERSION=lts
ARG ALPINE_VERSION=3.16
FROM node:${NODE_MAJOR_VERSION}-alpine${ALPINE_VERSION}

COPY ./version-info /usr/bin

RUN chmod +x /usr/bin/version-info
