FROM circleci/php:7.3-apache-node-browsers

USER root

RUN set -x ;   \
  apt-get update ; \
  sudo apt-get -y install libpng-dev mariadb-client xvfb ; \
  docker-php-ext-install gd mbstring mysqli pdo pdo_mysql ldap ; \
  \
  sudo apt-get -y install libldap2-dev ; \
  docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ ; \
  docker-php-ext-install ldap ;\
  apt-get purge -y --auto-remove libldap2-dev ;\
  \
  npm install -g gulp-cli ; \
  \
  apt-get clean --yes && \
  rm -rf \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/* ;

USER circleci

CMD ["/bin/sh"]
