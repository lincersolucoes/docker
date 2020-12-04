FROM odoo:12.0
MAINTAINER Hugo Rebelo <hugo.rebelo@indigoway.com>
USER root

# Install some deps, lessc and less-plugin-clean-css, and wkhtmltopdf
RUN set -x; \
        apt-get update \
        && apt-get install -y --no-install-recommends \
            build-essential \
            imagemagick \
            libblas-dev \
            libcairo2-dev \
            libcups2-dev \
            libffi-dev \
            libgeoip-dev \
            liblapacke-dev \
            libldap2-dev \
            libsasl2-dev \
            libssl-dev \
            libtiff5-dev \
            libxml2-dev \
            libxmlsec1-dev \
            libxslt1-dev \
            locales \
            pkg-config \
            poppler-utils \
            python3-dev \
            python3-gevent \
            python3-lasso \
            python3-pandas \
            python3-socks \
            python3-renderpm \
            python3-scipy \
            python3-simplejson \
            python3-sklearn \
            python3-watchdog \
            tesseract-ocr \
            tesseract-ocr-por \
            xvfb \
            unixodbc-dev


RUN set -x; \
        pip3 install --no-cache-dir \ 
            Markdown==3.2.1 \
            pathlib==1.0.1 \
            Num2Words==0.5.10 \
            awscli==1.18.45 \
            boto3==1.12.45 \
            boto==2.49.0 \
            botocore==1.15.45 \
            fpdf==1.7.2 \
            ftpfs==0.2 \
            google-cloud \
            google-cloud-translate \
            google-cloud-vision \
            https://github.com/mattgwwalker/msg-extractor/archive/v0.19.zip \
            markdown2==2.3.8 \
            #pdfmerge \
            nltk==3.5 \
            number2words==0.1.3 \
            oauthlib==3.1.0 \
            paramiko==2.7.1 \
            pdf2image==1.12.1 \
            pdfkit==0.6.1 \
            pika==1.1.0 \
            psycogreen==1.0.2 \
            pycairo==1.19.1 \
            pyopenssl==19.1.0 \
            pysftp==0.2.9 \
            pytrustnfe3==1.0.53 \
            pytz \
            regex==2020.4.4 \
            rotate_backups_s3==0.3 \
            viivakoodi==0.8.0 \
            xlrd==1.2.0 \
            xmltodict==0.12.0 \
            zeep==3.4.0

RUN set -x; \
	pip3 install --no-cache-dir \
	fs==2.4.11 \
	idna==2.9

# Install locales
RUN set -x; \
        echo "pt_BR.UTF-8 UTF-8" >> /etc/locale.gen \
        && echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen \
        && locale-gen


# Set default user when running the container
USER odoo
