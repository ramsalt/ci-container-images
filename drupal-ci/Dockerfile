FROM juampynr/drupal8ci

RUN apt-get install -y jq

RUN apt-get autoremove -y ;\
    apt-get clean ;\
    rm -rf /var/lib/apt/lists/*
