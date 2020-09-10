FROM alpine:3.12
LABEL MAINTAINER="Pedro Rodrigues <pir.pedro@gmail.com>"

# Declare environment variables ------------------

ENV LC_ALL en_US.UTF-8
ENV LANG en_US.UTF-8

ENV BUILD_DEPS \
    cairo-dev \ 
    libxmu-dev \
    openjdk8-jre-base \ 
    pango-dev \
    tiff-dev \
    tk-dev \
    python3-dev \
    musl-dev \
    libffi-dev

ENV BUILD_DEV_DEPS \
    build-base \
    automake \
    autoconf \
    libxml2-dev \
    linux-headers \
    python3 \
    py3-pip \
    expat-dev

ENV PERSISTENT_DEPS \
    bash \
    sudo \
    R-mathlib \
    gcc \ 
    gfortran \
    icu-dev \
    libjpeg-turbo \
    libpng-dev \
    make \
    openblas-dev \
    pcre-dev \
    readline-dev \
    xz-dev \
    zlib-dev \
    bzip2-dev \
    curl-dev

# Install R and R-dev ------------------

RUN apk add --no-cache --virtual .build-deps $BUILD_DEPS \
    && apk add --no-cache --virtual .build-dev-deps $BUILD_DEV_DEPS \
    && apk add --no-cache --virtual .persistent-deps $PERSISTENT_DEPS \
    && apk add --no-cache R R-dev \
    && pip3 install -U wheel \
    && pip3 install -U radian \
    && apk del .build-deps

# udunits lib doesn't exist already in alpine package manager, but it's commonly
# used to compile some R packages
RUN mkdir -p /usr/local/src \
    && cd /usr/local/src \
    && wget -c https://www.unidata.ucar.edu/downloads/udunits/udunits-2.2.26.tar.gz -O - | tar -xz \
    && cd udunits-2.2.26 \
    && ./configure && make && make install && make clean

RUN mkdir -p /usr/share/doc/R/html \
    && R -e 'make.packages.html(lib.loc = .libPaths(), temp = FALSE, verbose = TRUE, docdir = R.home("doc"))' \
    && R -e 'install.packages(c("devtools", "Rcpp"), repos="https://cloud.r-project.org")'

RUN addgroup -g 1000 -S app && adduser -u 1000 -S app -G app -s /bin/bash \
    && adduser app wheel \
    && sed -e 's;^# \(%wheel.*NOPASSWD.*\);\1;g' -i /etc/sudoers \
    && mkdir -p /usr/src/app \
    && chown -R app:app /usr/src

WORKDIR /usr/src/app

CMD ["R"]