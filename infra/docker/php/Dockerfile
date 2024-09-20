FROM amazonlinux:2

# timezone environment
ENV TZ=Asia/Tokyo \
  # locale
  LANG=ja_JP.UTF-8 \
  LANGUAGE=ja_JP:ja \
  LC_ALL=ja_JP.UTF-8

RUN yum update -y \
  && yum install -y \
  glibc-langpack-ja \
  zip \
  unzip \
  openssl \
  mod_ssl \
  sudo \
  systemd \
  tar \
  vim \
  httpd

RUN amazon-linux-extras install -y epel
RUN yum install -y https://rpms.remirepo.net/enterprise/remi-release-7.rpm

RUN yum clean metadata \
  && yum install -y \
  php82 \
  php82-php \
  php82-php-mbstring \
  php82-php-pdo \
  php82-php-xml \
  php82-php-fpm \
  php82-php-mysqlnd \
  php82-php-gd \
  php82-php-opcache \
  php82-php-bcmath 

RUN alternatives --install /usr/bin/php php /usr/bin/php82 1


# Create the directories for SSL certificates
RUN mkdir -p /etc/pki/tls/certs /etc/pki/tls/private

# Generate a self-signed SSL certificate
RUN openssl req -x509 -nodes -days 365 \
    -newkey rsa:2048 \
    -keyout /etc/pki/tls/private/localhost.key \
    -out /etc/pki/tls/certs/localhost.crt \
    -subj "/C=US/ST=State/L=City/O=Organization/OU=Department/CN=localhost"

# composer のインストール
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1

# ファイルのコピー
COPY ./infra/docker/php/php.ini /etc/opt/remi/php81/php.ini
COPY ./infra/docker/apache/vhost.conf /etc/httpd/conf.d/vhost.conf
#COPY ./infra/docker/apache/rewrite.conf /etc/httpd/conf.modules.d/rewrite.conf

CMD [ "apachectl", "-D", "FOREGROUND" ]

WORKDIR /work/backend
