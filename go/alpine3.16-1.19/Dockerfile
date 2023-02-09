FROM golang:1.19.5-alpine3.16

WORKDIR /go/src/app

ENV GO111MODULE=on
ENV GOPROXY=https://goproxy.cn,direct
ENV GOLANG_VERSION 1.19.5

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
    set -eux; \
	apk update && \
	apk add --no-cache gcc g++ make nodejs npm musl-dev python3