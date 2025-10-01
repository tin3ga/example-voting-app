ARG VERSION=3.13-slim

FROM python:${VERSION} AS builder

# Install required system dependencies
RUN apt-get update && apt-get install -y \
    curl \
    gcc \
    build-essential \
    python3-venv \
    python3-dev \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://install.python-poetry.org | python3 -

ENV PATH="/root/.local/bin:$PATH"

RUN poetry config virtualenvs.in-project true

WORKDIR /app/vote

COPY pyproject.toml poetry.lock ./

RUN poetry install


# Development Stage
FROM python:${VERSION} AS development

LABEL org.opencontainers.image.authors="tin3ga"
LABEL org.opencontainers.image.description="Container image for https://github.com/tin3ga/example-voting-app vote service"

RUN apt-get update && apt-get install -y curl

WORKDIR /app/vote

COPY --from=builder /app/vote .

ENV PATH="/app/vote/.venv/bin:$PATH"

ENV PYTHONPATH="/app/vote"

COPY . ./

EXPOSE 80

CMD [ "python", "app.py"]


# # Development Stage
FROM python:${VERSION} AS production

LABEL org.opencontainers.image.authors="tin3ga"
LABEL org.opencontainers.image.description="Container image for https://github.com/tin3ga/example-voting-app vote service"

RUN apt-get update && apt-get install -y curl

WORKDIR /app/vote

COPY --from=builder /app/vote .

ENV PATH="/app/vote/.venv/bin:$PATH"

ENV PYTHONPATH="/app/vote"

COPY . ./

EXPOSE 80

CMD ["gunicorn", "app:app", "-b", "0.0.0.0:80", "--log-file", "-", "--access-logfile", "-", "--workers", "4", "--keep-alive", "0"]