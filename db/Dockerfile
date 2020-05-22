FROM postgres:12

RUN set -eux; \
        \
        apt-get update && apt-get install -y --no-install-recommends \
                ca-certificates \
                wget \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
        mkdir pgroonga-work; \
        cd pgroonga-work; \
        \
        wget https://packages.groonga.org/debian/groonga-archive-keyring-latest-buster.deb; \
        apt-get install -y ./groonga-archive-keyring-latest-buster.deb; \
        apt-get update && apt-get install -y --no-install-recommends \
                groonga-tokenizer-mecab \
                postgresql-12-pgroonga \
        ; \
        rm -rf /var/lib/apt/lists/*; \
        \
        cd ..; \
        rm -rf pgroonga-work;
