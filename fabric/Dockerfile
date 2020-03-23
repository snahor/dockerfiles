FROM python:2.7.17-alpine3.11

ARG FABRIC_VERSION=1.14.1

RUN apk add --no-cache --virtual .build-deps \
      musl-dev \
      libffi-dev \
      openssl-dev \
      gcc \
      make \
      ; \
      \
    { \
      echo '[global]'; \
      echo 'no-python-version-warning = true'; \
    } > /etc/pip.conf; \
    \
    pip install --no-cache-dir \
      fabric==$FABRIC_VERSION \
      ; \
    \
    apk del .build-deps
