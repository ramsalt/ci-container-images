ARG PHP_VERSION=7.4
ARG DRUPAL_IMAGE_TAG=9

FROM drupal:${DRUPAL_IMAGE_TAG}-php${PHP_VERSION}-apache

ENV PHP_VERSION=${PHP_VERSION}
ENV DRUPAL_IMAGE_TAG=${DRUPAL_IMAGE_TAG}

USER root

WORKDIR /tmp

RUN  set -ex; \
    # Remove the default drupal installation, leaving an empty directory
    rm -rf /opt/drupal && mkdir /opt/drupal ; \
    apt-get update ; \
    apt-get install --yes \
        gnupg2 \
        apt-transport-https \
        git \
        imagemagick \
        libmagickwand-dev \
        libgmp-dev \
        libldap2-dev \
        libpng-dev \
        libwebp-dev \
        libtidy-dev \
        libiconv-hook-dev \
        libbz2-dev \
        libmcrypt-dev \
        libyaml-dev \
        libxml2-dev \
        libxslt1-dev \
        libzip-dev \
        mariadb-client \
        rsync \
        sudo \
        unzip \
        vim \
        wget \
        tidy \
        jq \
        sqlite3 \
        ; \
    \
    # Install Chrome browser repos.
    wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add -  ; \
    sh -c 'echo "deb https://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list'  ; \
    \
    apt-get update ; \
    apt-get install --yes \
        google-chrome-unstable \
        ; \
    \
    docker-php-ext-install \
        bcmath \
        bz2 \
        exif \
        gmp \
        intl \
        mysqli \
        opcache \
        pdo_mysql \
        sockets \
        tidy \
        xsl \
        zip \
    ; \
    pecl install \
        redis \
        xdebug \
        # imagick \
    ; \
    \
    if [ "$(printf %.1s $PHP_VERSION)" = '8' ]; then \
        # Imagick dev
        cd /tmp; \
        git clone https://github.com/Imagick/imagick; \
        cd imagick; \
        git checkout 132a11fd26675db9eb9f0e9a3e2887c161875206; \
        phpize; \
        ./configure; \
        make; \
        make install; \
    ; \
    else \
        pecl install imagick; \
    fi; \
    \
    docker-php-ext-enable \
        redis \
        xdebug \
        imagick \
    ; \
    \
    echo 'memory_limit = -1' > /usr/local/etc/php/php-cli.ini ; \
    \
    # Composer
    wget -qO- https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer; \
    \
    # Drupal console
    console_url="https://github.com/hechoendrupal/drupal-console-launcher/releases/download/1.9.7/drupal.phar"; \
    curl "${console_url}" -L -o drupal.phar; \
    mv drupal.phar /usr/local/bin/drupal; \
    chmod +x /usr/local/bin/drupal; \
    \
    # Install Robo CI.
    wget -qO /usr/local/bin/robo https://robo.li/robo.phar && chmod +x /usr/local/bin/robo ; \
    \
    apt-get autoremove -y ;\
    apt-get autoclean -y ;\
    apt-get clean -y ;\
    rm -rf /var/lib/apt/lists/*

COPY files/ImageMagick-6/*.xml /etc/ImageMagick-6/

WORKDIR /opt/drupal
