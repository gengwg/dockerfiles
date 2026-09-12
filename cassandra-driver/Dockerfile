# This Dockerfile demonstrates how to use a pip proxy
FROM python:3.14-slim
LABEL org.opencontainers.image.authors="gengwg"

ARG HTTP_PROXY=http://proxy.example.com:8080
# we need also https, because during install of cassandra driver
# it goes out to an https site for extra resources
ARG HTTPS_PROXY=http://proxy.example.com:8080
RUN pip install --no-cache-dir --proxy "$HTTP_PROXY" --upgrade cassandra-driver==3.30.1