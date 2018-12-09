FROM debian:jessie
MAINTAINER Hugo Rebelo <hugo.rebelo@indigoway.com>

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            build-essential \
            ca-certificates \
            curl \
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
            libxslt1-dev \
            node-less \
            poppler-utils \
            python \
            python-all-dev \
            python-dev \
            python-gevent \
            python-pandas \
            python-pip \
            python-pysocks \
            python-renderpm \
            python-scipy \
            python-simplejson \
            python-sklearn \
            python-support \
            python-watchdog \
            tesseract-ocr \
            tesseract-ocr-por \
            unixodbc-dev \
        && curl -o wkhtmltox.deb -SL http://nightly.odoo.com/extra/wkhtmltox-0.12.1.2_linux-jessie-amd64.deb \
        && echo '40e8b906de658a2221b15e4e8cd82565a47d7ee8 wkhtmltox.deb' | sha1sum -c - \
        && dpkg --force-depends -i wkhtmltox.deb \
        && apt-get -y install -f --no-install-recommends \
        && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false npm \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb \
        && pip install --upgrade pip setuptools 

RUN set -x; \
        pip install --no-cache-dir \ 
            Markdown==3.0.1 \
            Num2Words==0.5.7 \
            awscli==1.16.14 \
            beautifulsoup4==4.4.1 \
            boto3==1.9.4 \
            boto==2.49.0 \
            botocore==1.12.4 \
            fpdf==1.7.2 \
            ftpfs==0.2 \
            google-cloud \
            google-cloud-translate \
            google-cloud-vision \
            grpc \
            https://github.com/mattgwwalker/msg-extractor/archive/v0.19.zip \
            markdown2==2.3.6 \
            nltk==3.1 \
            paramiko \
            pdfmerge==0.0.7 \
            psycogreen==1.0 \
            pyopenssl \
            pytz \
            regex==2018.8.29 \
            six==1.11.0 \
            viivakoodi==0.8 \
            xlrd==1.1.0 \
            xmltodict==0.11.0 

# Extra libraries
COPY ./eggs/ /eggs/
RUN set -x; \
        easy_install /eggs/fs-0.5.4-py2.7.egg

# Install Odoo
ENV ODOO_VERSION 10.0
ENV ODOO_RELEASE 20181003
RUN set -x; \
        curl -o odoo.deb -SL http://downloads.intelligenti.com.br.s3.amazonaws.com/odoo/odoo_${ODOO_VERSION}.${ODOO_RELEASE}_all.deb \
        && dpkg --force-depends -i odoo.deb \
        && apt-get update \
        && apt-get -y install -f --no-install-recommends \
        && rm -rf /var/lib/apt/lists/* odoo.deb

# Copy entrypoint script and Odoo configuration file
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
