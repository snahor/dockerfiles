ARG ALPINE_VERSION=3.14.2
FROM alpine:$ALPINE_VERSION AS base

FROM base AS builder
ARG HAPROXY_VERSION=2.4.8
ARG LUA_VERSION=5.4
RUN apk add --no-cache \
        alpine-sdk \
        linux-headers \
        openssl-libs-static \
        openssl-dev \
        pcre-dev \
        lua$LUA_VERSION-dev
RUN curl -sL -o /src.tar.gz https://www.haproxy.org/download/${HAPROXY_VERSION%.*}/src/haproxy-$HAPROXY_VERSION.tar.gz && \
    mkdir /src && cd /src && tar -xf /src.tar.gz --strip-components=1 && \
    make TARGET=linux-musl \
         USE_THREAD=1 \
         USE_PROMEX=1 \
         USE_OPENSSL=1 \
         USE_TFO=1 \
         USE_NS=1 \
         USE_PCRE=1 \
         USE_PCRE_JIT=1 \
         USE_LUA=1 \
         LUA_INC=/usr/include/lua$LUA_VERSION \
         LUA_LIB=/usr/lib/lua$LUA_VERSION \
         LDFLAGS=-static \
         -j $(nproc)

FROM base AS release
COPY --from=builder /src/haproxy /usr/local/bin/haproxy