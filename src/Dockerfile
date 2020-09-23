FROM python:alpine

LABEL \
    maintainer="Morgan AUCHEDE <morgan.auchede@gmail.com>"

RUN set -e -u -x \
\
    # Install packages
\
    && apk add --no-cache --no-progress --virtual BUILD_DEPS acl-dev build-base curl linux-headers openssl-dev \
    && apk add --no-cache --no-progress bash ca-certificates libacl \
\
    # Install borg
\
    && pip install borgbackup \
\
    # Install borgmatic
\
    && pip install borgmatic \
\
    # Install dumb-entrypoint
\
    && curl --location "https://gitlab.com/timonier/dumb-entrypoint/raw/master/src/dumb-entrypoint/installer" | bash -s -- install \
    && curl --location "https://gitlab.com/timonier/dumb-entrypoint/raw/master/src/gosu/installer" | bash -s -- install \
\
    # Install s3cmd
\
    && pip install s3cmd \
\
    # Clean
\
    && apk del --no-progress BUILD_DEPS \
    && rm -f -r /tmp/*

ENTRYPOINT [ "dumb-entrypoint" ]
