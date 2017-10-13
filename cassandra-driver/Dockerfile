# This Dockerfile demostates how to use pip proxy
FROM python:3.4.5
MAINTAINER gengwg

ENV HTTP_PROXY=http://example.com:8080
# we need also https, because during install of cassandra driver
# it goes out to an https site for extra resources
ENV HTTPS_PROXY=http://example.com:8080
RUN pip install --proxy $HTTP_PROXY --upgrade cassandra-driver==3.7.1
