FROM public.ecr.aws/docker/library/python:3.10-alpine3.19

# Prevent GCC 14+ and sub-shells from failing on legacy C pointer warnings
ENV CFLAGS="-Wno-error=incompatible-pointer-types"

RUN adduser -u 1000 -D app

RUN apk update && \
  apk add --no-cache \
  pcre \
  pcre-dev \
  build-base \
  gcc \
  linux-headers \
  openssl \
  libffi-dev \
  && pip install pipenv

RUN mkdir -p /var/www/app && mkdir /app

WORKDIR /app

COPY Pipfile* /app/

RUN pipenv install --system --deploy

COPY . /app

CMD ["flask", "--app", ".", "run", "--host=0.0.0.0", "--port=5000"]