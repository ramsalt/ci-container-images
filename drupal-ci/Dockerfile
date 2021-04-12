ARG DRUPAL_IMAGE_TAG

FROM drupal:${DRUPAL_IMAGE_TAG}-apache

USER root

RUN  set -ex; \
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
