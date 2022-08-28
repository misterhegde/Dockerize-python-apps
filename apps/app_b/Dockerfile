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
WORKDIR /usr/app/venv
COPY --from=build /usr/app/venv .
COPY . .


ENV PATH="/usr/app/venv/bin:${PATH}"


# CMD [ "python", "app_b.py" ]
CMD [ "gunicorn", "--bind", "0.0.0.0:5001", "app_b:application" ]




