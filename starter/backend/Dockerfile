FROM public.ecr.aws/docker/library/python:3.10-alpine

ENV FLASK_RUN_HOST=0.0.0.0

WORKDIR /

RUN adduser -u 1000 -D app

RUN apk update && \
    apk add \
    pcre \
    pcre-dev \
    build-base \
    gcc \
    linux-headers \
    openssl \
    libffi-dev

# Use a fixed Pipenv version compatible with the project's old lock file.
RUN pip install --no-cache-dir pipenv==2023.2.4

RUN mkdir -p /var/www/app && \
    mkdir /app

COPY Pipfile Pipfile.lock /app/

RUN cd /app && \
    pipenv install --system --deploy

COPY . /app

CMD ["uwsgi", "--http", ":5000", "--master", "--enable-threads", "-s", "/var/www/app/app.sock", "--manage-script-name", "--mount", "/=app:app"]