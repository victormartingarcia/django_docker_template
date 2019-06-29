FROM python:3.7-alpine

# set environment variables
ENV PYTHONDONTWRITEBYTECODE 1
ENV PYTHONUNBUFFERED 1

# install psycopg2
RUN apk update \
    && apk add --virtual build-deps gcc python3-dev musl-dev \
    && apk add postgresql-dev \
    && pip install psycopg2 \
    && apk del build-deps

COPY . /backend
WORKDIR /backend
RUN pip install -r requirements.txt

# run entrypoint.sh
ENTRYPOINT ["/django/entrypoint.sh"]
