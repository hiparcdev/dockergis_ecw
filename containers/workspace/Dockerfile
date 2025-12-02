FROM ubuntu:24.04

# Evita prompts interativos durante a instalação
ENV DEBIAN_FRONTEND=noninteractive

# Define a linguagem padrão
ARG LOCALE=en_US.UTF-8
RUN apt-get update && \
    apt-get install -y locales && \
    locale-gen $LOCALE && \
    update-locale LANG=$LOCALE

# Define a timezone
ARG TZ=UTC
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Instala ferramentas básicas e PHP do repositório oficial
RUN apt-get update && \
    apt-get install -y \
    curl \
    wget \
    git \
    vim \
    nano \
    tree \
    rsync \
    sqlite3 \
    zip \
    unzip \
    php-cli \
    php-curl \
    php-xml \
    php-mbstring \
    php-mysql \
    php-pgsql \
    php-sqlite3 \
    php-zip \
    php-bcmath \
    php-gd \
    php-intl \
    postgresql-client && \
    apt-get clean

# Instala Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Instala Node.js LTS
RUN curl -fsSL https://deb.nodesource.com/setup_18.x | bash - && \
    apt-get install -y nodejs && \
    apt-get clean

# Adiciona um usuário não-root
ARG PUNAME=workspace
ARG PUID=1000
ARG PGNAME=workspace
ARG PGID=1000

RUN groupadd -g ${PGID} ${PGNAME} && \
    useradd -l -u ${PUID} -g ${PGID} -m ${PUNAME} && \
    usermod -p "*" ${PUNAME} -s /bin/bash

# Configura Composer para o usuário root
RUN composer self-update --2

# Configura variáveis de ambiente
ENV PATH="/var/www/vendor/bin:${PATH}"
RUN echo 'export PATH="/var/www/vendor/bin:$PATH"' >> /root/.bashrc
RUN echo 'export PATH="$HOME/.composer/vendor/bin:$PATH"' >> /root/.bashrc

# Limpa arquivos temporários
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Define o workdir padrão
WORKDIR /var/www