# -*- mode: dockerfile; coding: utf-8 -*-
FROM debian:buster-slim AS build
RUN apt-get update && apt-get -y --no-install-recommends install \
      build-essential file texinfo git ca-certificates \
      libffi-dev libgc-dev libgmp-dev libpcre3-dev \
      autoconf automake libtool m4 \
 && rm -rf /var/lib/apt/lists/*
WORKDIR /build
RUN git clone https://github.com/egallesio/STklos.git --depth 1 --tags
WORKDIR /build/stklos
RUN autoreconf
RUN ./configure \
  --without-provided-bignum --without-provided-gc --without-provided-regexp
RUN make
RUN make install

FROM debian:buster-slim
RUN apt-get update && apt-get -y --no-install-recommends install \
      libffi6 libgc1c2 libgmp10 libpcre3 libreadline7 \
 && rm -rf /var/lib/apt/lists/*
COPY --from=build /usr/local/ /usr/local/
COPY scheme-banner /usr/local/bin/scheme-banner
RUN ln -s libreadline.so.7.0 /lib/x86_64-linux-gnu/libreadline.so
CMD ["scheme-banner"]
