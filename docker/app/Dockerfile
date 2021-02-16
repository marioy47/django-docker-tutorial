# docker/app/Dockerfile
FROM python:3.9-alpine

ENV PYTHONUNBUFFERED 1

RUN mkdir -p /app \
    && apk add python3-dev build-base linux-headers pcre-dev \
        jpeg-dev zlib-dev \
        libffi-dev libressl-dev rust cargo \
        postgresql-dev \
    && pip install --no-cache-dir pipenv django

WORKDIR /app

