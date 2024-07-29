# syntax=docker/dockerfile:1

# use latest python 3 alpine image.
FROM python:3-alpine as base

# install system dependencies.
RUN apk update && \
    apk add --no-cache bash go curl fontconfig

# install go package.
RUN go install github.com/mingrammer/round@latest

# install fonts
RUN curl -O https://noto-website.storage.googleapis.com/pkgs/NotoSansCJKjp-hinted.zip \
    && mkdir -p /usr/share/fonts/NotoSansCJKjp \
    && unzip NotoSansCJKjp-hinted.zip -d /usr/share/fonts/NotoSansCJKjp/ \
    && rm NotoSansCJKjp-hinted.zip \
    && fc-cache -fv

FROM python:3-alpine as copy-workspace

COPY --from=base /root/go/bin/round /usr/local/bin/round
COPY --from=base /usr/share/fonts/NotoSansCJKjp /usr/share/fonts/NotoSansCJKjp

# install system dependencies.
RUN apk update && \
    apk add --no-cache --virtual .build gcc libc-dev g++ git bash imagemagick inkscape curl xdg-utils && \
    apk add --no-cache graphviz && \
    # install python requirements.
    pip --no-cache-dir install --upgrade pip && \
    pip --no-cache-dir install black graphviz jinja2 diagrams && \
    apk del .build

# project directory.
# WORKDIR /usr/src/diagrams
WORKDIR /workspace

# Copy the rest of your app's source code from your host to your image filesystem.
COPY . .
