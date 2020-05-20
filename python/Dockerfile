FROM python:3.6.8-alpine3.8
ENV PYTHONUNBUFFERED 1
RUN df -h 
RUN apk update && \
    apk add --no-cache linux-headers libc-dev gcc postgresql-libs postgresql-dev && \
    rm -rf /usr/local/bundle/cache/* /usr/local/share/.cache/* /var/cache/* /tmp/* /usr/lib/mysqld* /usr/bin/mysql*
RUN mkdir /code
WORKDIR /code
ADD requirements.txt /code/
RUN pip install --upgrade pip && pip install -r requirements.txt
# RUN apk del libxml2-dev curl-dev make gcc libc-dev g++
ADD . /code/
