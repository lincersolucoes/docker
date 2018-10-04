FROM debian:jessie
MAINTAINER Hugo Rebelo <hugo.rebelo@indigoway.com>

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            #build-essential \
            ca-certificates \
            curl \
            node-less \
            #python-dev \
            python-fs \
            python-gevent \
            python-pandas \
            python-pip \
            python-pypdf2 \
            python-pysocks \
            python-regex \
            python-renderpm \
            python-simplejson \
            python-sklearn \
            python-support \
            python-watchdog \
        && curl -o wkhtmltox.deb -SL http://nightly.odoo.com/extra/wkhtmltox-0.12.1.2_linux-jessie-amd64.deb \
        && echo '40e8b906de658a2221b15e4e8cd82565a47d7ee8 wkhtmltox.deb' | sha1sum -c - \
        && dpkg --force-depends -i wkhtmltox.deb \
        && apt-get -y install -f --no-install-recommends \
        && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false -o APT::AutoRemove::SuggestsImportant=false npm \
        && rm -rf /var/lib/apt/lists/* wkhtmltox.deb \
        && pip install \
            fpdf==1.7.2 \
            ftpfs==0.2 \
            s3fs==0.1.6 \
            https://github.com/mattgwwalker/msg-extractor/zipball/master \
            psycogreen==1.0
            #regex

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
