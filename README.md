# Dockerfiles

Dockerfile examples

## ARG and FROM interact

https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact

An ARG declared before a FROM is outside of a build stage, so it can’t be used in any instruction after a FROM. To use the default value of an ARG declared before the first FROM use an ARG instruction without a value inside of a build stage:

```
ARG VERSION=latest
FROM busybox:$VERSION
ARG VERSION
RUN echo $VERSION > image_version
```

Here is a simple test.

Before (comment out ARG line):

```
$ cat Dockerfile
ARG VERSION=v1.6.1

FROM golang:1.17
#ARG VERSION

WORKDIR /

RUN echo "My version: $VERSION"
```

Build:

```
$ docker build -t testarg . --no-cache
Sending build context to Docker daemon  161.8MB
Step 1/4 : ARG VERSION=v1.6.1
Step 2/4 : FROM golang:1.17
 ---> 0659a535a734
Step 3/4 : WORKDIR /
 ---> Running in d602e74033f5
Removing intermediate container d602e74033f5
 ---> 59b28e6e8134
Step 4/4 : RUN echo "My kyverno version: $VERSION"
 ---> Running in 763453b4e253
My version: # <---------- No version output!
Removing intermediate container 763453b4e253
 ---> 427e829734a1
Successfully built 427e829734a1
Successfully tagged testarg:latest
```

After (uncomment ARG line):

```
$ vim Dockerfile
$ cat Dockerfile
ARG VERSION=v1.6.1

FROM golang:1.17
ARG VERSION

WORKDIR /

RUN echo "My version: $VERSION"
```

Build:

```
$ docker build -t testarg . --no-cache
....
Step 5/5 : RUN echo "My version: $VERSION"
 ---> Running in 1a7988157b25
My version: v1.6.1 # <---------- has version
....
Successfully tagged testarg:latest
```

Another way is simply put ARGs after FROM, if you don't use the ARG in the FROM statement.

```
$ cat Dockerfile
FROM golang:1.17
ARG VERSION=v1.6.1

WORKDIR /

RUN echo "My kyverno version: $VERSION"

$ docker build -t k8sdebug . --no-cache
...
Step 4/4 : RUN echo "My kyverno version: $VERSION"
 ---> Running in 41485c0367d3
My version: v1.6.1 # <---- this also works
```
