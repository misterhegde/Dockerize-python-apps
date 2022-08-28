# A multi-stage build process.
FROM python:3-slim as build
RUN apt-get update
RUN apt-get install -y --no-install-recommends \
build-essential gcc 

WORKDIR /usr/app
RUN python -m venv /usr/app/venv
ENV PATH="/usr/app/venv/bin:${PATH}"

COPY requirements.txt .
RUN pip install -r requirements.txt


FROM python:3-slim@sha256:dff7fd9200421a8c65e020af221a21c8aab784c5c8a8d55c64a095b645209d77
RUN apt-get update; apt-get install curl -y
RUN groupadd -g 999 python && \
    useradd -r -u 999 -g python python

RUN mkdir /usr/app && chown python:python /usr/app

WORKDIR /usr/app/venv
COPY --chown=python:python --from=build /usr/app/venv ./venv
COPY --chown=python:python . .
USER 999



COPY --from=build /usr/app/venv .
COPY . .


ENV PATH="/usr/app/venv/bin:${PATH}"


# python wsgi for production
CMD [ "gunicorn", "--bind", "0.0.0.0:5000", "app_a:application", "--timeout 90" ]

HEALTHCHECK --interval=15s --timeout=30s --retries=3 CMD curl -f https://localhost:5000/hello





