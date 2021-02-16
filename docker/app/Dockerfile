# docker/app/Dockerfile
FROM python:3.9-alpine

LABEL maintainer="Mario Yepes marioyepes.com"

ENV PYTHONUNBUFFERED 1
ENV PROJECT_DIR /app/

COPY . ${PROJECT_DIR}
COPY --chmod=755 docker/app/entrypoint.sh /entrypoint.sh

WORKDIR ${PROJECT_DIR}
EXPOSE 8000 8080

RUN apk add --update --no-cache --virtual .tmp \
        python3-dev build-base linux-headers pcre-dev \
        jpeg-dev zlib-dev \
        postgresql-dev \
    && pip install --no-cache-dir pipenv \
    && pipenv install --system --deploy \
    && pipenv --clear \
    && find /usr/local \
        \( -type d -a -name test -o -name tests \) \
        -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
        -exec rm -rf '{}' + \
    && runDeps="$( \
        scanelf --needed --nobanner --recursive /usr/local \
                | awk '{ gsub(/,/, "\nso:", $2); print "so:" $2 }' \
                | sort -u \
                | xargs -r apk info --installed \
                | sort -u \
    )" \
    && apk add --virtual .rundeps $runDeps \
    && apk del .tmp \
    && adduser -D django 


USER django

ENTRYPOINT ["/entrypoint.sh"]
CMD ["uwsgi", "--http", "0.0.0.0:8080", "--master", "--enable-threads", "--module", "config.wsgi"]

