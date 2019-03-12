FROM debian:stretch
LABEL maintainer="Hugo Rebelo <hugo.rebelo@indigoway.com>"

# Generate locale C.UTF-8 for postgres and general locale data
ENV LANG C.UTF-8

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            #libssl1.0-dev \
            build-essential \
            ca-certificates \
            curl \
            dirmngr \
            fonts-noto-cjk \
            gnupg \
            imagemagick \
            libblas-dev \
            libcups2-dev \
            libffi-dev \
            libgeoip-dev \
            liblapacke-dev \
            libldap2-dev \
            libpq-dev \
            libsasl2-dev \
            libssl-dev \
            libtiff5-dev \
            libxml2-dev \
            libxmlsec1-dev \
            libxslt1-dev \
            locales \
            node-less \
            pkg-config \
            python3-dev \
            python3-pip \
            python3-pyldap \
            python3-qrcode \
            python3-renderpm \
            python3-setuptools \
            python3-vobject \
            python3-watchdog \
            python3-wheel \
            xz-utils \
        && locale-gen pt_BR.UTF-8 \
        && curl -o wkhtmltox.deb -sSL https://github.com/wkhtmltopdf/wkhtmltopdf/releases/download/0.12.5/wkhtmltox_0.12.5-1.stretch_amd64.deb \
        && echo '7e35a63f9db14f93ec7feeb0fce76b30c08f2057 wkhtmltox.deb' | sha1sum -c - \
        && dpkg --force-depends -i wkhtmltox.deb\
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb

# install latest postgresql-client
RUN set -x; \
        echo 'deb http://apt.postgresql.org/pub/repos/apt/ stretch-pgdg main' > etc/apt/sources.list.d/pgdg.list \
        && export GNUPGHOME="$(mktemp -d)" \
        && repokey='B97B0AFCAA1A47F044F244A07FCC7D46ACCC4CF8' \
        && gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "${repokey}" \
        && gpg --armor --export "${repokey}" | apt-key add - \
        && gpgconf --kill all \
        && rm -rf "$GNUPGHOME" \
        && apt-get update  \
        && apt-get install -y postgresql-client \
        && rm -rf /var/lib/apt/lists/*

# Install Odoo
ENV ODOO_VERSION 11.0
ARG ODOO_RELEASE=20190128
ARG ODOO_SHA=30cb83ce38ec0b4a459e5590407cb2f9dcf46061
RUN set -x; \
        curl -o odoo.deb -sSL http://nightly.odoo.com/${ODOO_VERSION}/nightly/deb/odoo_${ODOO_VERSION}.${ODOO_RELEASE}_all.deb \
        && echo "${ODOO_SHA} odoo.deb" | sha1sum -c - \
        && dpkg --force-depends -i odoo.deb \
        && apt-get update \
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* odoo.deb

# Copy entrypoint script and Odoo configuration file
RUN pip3 install num2words xlwt \
            #grpc \
            #pdfmerge \
            Markdown==3.0.1 \
            awscli==1.16.14 \
            beautifulsoup4==4.4.1 \
            boto3==1.9.4 \
            boto==2.49.0 \
            botocore==1.12.4 \
            extract-msg==0.22.1 \
            fpdf==1.7.2 \
            ftpfs==0.2 \
            google-cloud \
            google-cloud-translate \
            google-cloud-vision \
            grpcio \
            https://github.com/mattgwwalker/msg-extractor/archive/v0.19.zip \
            markdown2==2.3.6 \
            nltk==3.1 \
            num2words \
            oauthlib \
            ofxparse==0.16 \
            panda==0.3.1 \
            pandas==0.24.1 \
            paramiko \
            pdf2image \
            phonenumbers \
            pillow==3.4.2 \
            psycogreen==1.0 \
            pycnab240 \
            pyopenssl \
            pysftp \
            python3-boleto \
            python3-cnab \
            pytrustnfe3>=0.10.0 \
            pytz \
            regex==2018.8.29 \
            report==0.0.1 \
            rotate_backups_s3 \
            simplejson==3.16.0 \
            six==1.11.0 \
            suds-jurko-requests \
            viivakoodi==0.8 \
            wheel \
            xlrd==1.1.0 \
            xlwt \
            xmlsec \
            xmltodict==0.11.0

COPY ./entrypoint.sh /
COPY ./odoo.conf /etc/odoo/
RUN chown odoo /etc/odoo/odoo.conf

# Mount /var/lib/odoo to allow restoring filestore and /mnt/extra-addons for users addons
RUN mkdir -p /mnt/extra-addons \
        && chown -R odoo /mnt/extra-addons
VOLUME ["/var/lib/odoo", "/mnt/extra-addons"]

# Expose Odoo services
EXPOSE 8069 8071

# Set the default config file
ENV ODOO_RC /etc/odoo/odoo.conf

# Set default user when running the container
USER odoo

ENTRYPOINT ["/entrypoint.sh"]
CMD ["odoo"]
