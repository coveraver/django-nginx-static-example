FROM python:3.9-slim AS server

WORKDIR /server

RUN set -ex; \
  apt-get update; \
  apt-get install -y \
    curl \
    git \
    libpq-dev \
    postgresql-client \
  --no-install-recommends \
  --no-install-suggests

COPY pyproject.toml poetry.lock ./

RUN set -ex; \
  pip install poetry; \
  poetry config virtualenvs.create false; \
  poetry install --no-root --no-dev

COPY . .

RUN set -ex; \
  python manage.py collectstatic \
    --no-input

FROM nginx AS proxy

COPY --from=server server/nginx/nginx.conf /etc/nginx/conf.d/default.conf
COPY --from=server server/staticfiles /var/www/static