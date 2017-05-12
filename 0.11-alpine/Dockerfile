FROM alpine:3.5

# Fluent Bit version
ENV FLB_MAJOR 0
ENV FLB_MINOR 11
ENV FLB_PATCH 0
ENV FLB_VERSION 0.11.4

ENV FLB_TARBALL https://github.com/fluent/fluent-bit/archive/v$FLB_VERSION.tar.gz

RUN mkdir -p /fluent-bit/bin /fluent-bit/etc

RUN apk --no-cache --virtual .build-deps add \
        gcc \
        make \
        cmake \
        build-base \
        abuild \
        binutils \
        zlib-dev \
        linux-headers \
    && wget -O "/tmp/fluent-bit-${FLB_VERSION}.tar.gz" ${FLB_TARBALL} \
    && cd /tmp && tar xzf "fluent-bit-$FLB_VERSION.tar.gz" \
    && cd "fluent-bit-$FLB_VERSION"/build/ \
    && cmake -DFLB_DEBUG=On -DFLB_TRACE=On -DFLB_JEMALLOC=On -DFLB_BUFFERING=On ../ \
    && make \
    && install bin/fluent-bit /fluent-bit/bin/ \
    && apk del .build-deps \
    && rm -rf /tmp/*

# Configuration files
COPY fluent-bit.conf /fluent-bit/etc/
COPY parsers.conf /fluent-bit/etc/

# Entry point
CMD ["/fluent-bit/bin/fluent-bit", "-c", "/fluent-bit/etc/fluent-bit.conf"]
