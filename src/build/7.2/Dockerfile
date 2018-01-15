FROM php:7.2-fpm

ARG LANGUAGE_CODE="${LANGUAGE_CODE:-en}"
ARG COUNTRY_CODE="${COUNTRY_CODE:-GB}"
ARG ENCODING="${ENCODING:-UTF-8}"

ARG LOCALE_STRING="${LANGUAGE_CODE}_${COUNTRY_CODE}"
ARG LOCALIZATION="${LOCALE_STRING}.${ENCODING}"

ARG CONTAINER_USER=php-fpm
ARG FPM_CONF=/usr/local/etc/php-fpm.d/www.conf

RUN apt-get update && \
    apt-get -y upgrade && \
    apt-get -y install \
        locales \
        libsodium-dev \
        zip \
        unzip && \
    docker-php-ext-install \
        sockets \
        pdo_mysql \
        sodium && \

    # Locales
    echo "${LOCALIZATION} ${ENCODING}" > /etc/locale.gen && \
    locale-gen "${LOCALIZATION}" && \

    # Create new user
    useradd -m -u 1000 -s /usr/sbin/nologin ${CONTAINER_USER} && \
    sed -i \
        -e "s/user = www-data/user = ${CONTAINER_USER}/g" \
        -e "s/group = www-data/group = ${CONTAINER_USER}/g" \
        "${FPM_CONF}"

COPY ./php-fpm.d/zz-log.conf /usr/local/etc/php-fpm.d/zz-log.conf

USER "${CONTAINER_USER}"

ENV LANG "${LOCALIZATION}"
ENV LANGUAGE "${LOCALE_STRING}:${LANGUAGE_CODE}"
ENV LC_ALL "${LOCALIZATION}"
