FROM python:3.10-slim-buster
# Install python packages
COPY requirements.txt /tmp
RUN apt-get update \
    && apt-get -y install libpq-dev gcc \
    && pip install -r /tmp/requirements.txt