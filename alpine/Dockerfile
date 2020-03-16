ARG ALPINE_VERSION
FROM alpine:${ALPINE_VERSION}

# 替换国内镜像源，包含main、community软件
RUN echo "http://mirrors.aliyun.com/alpine/v3.9/main/" > /etc/apk/repositories && \
	echo "http://mirrors.aliyun.com/alpine/v3.9/community/" >> /etc/apk/repositories

RUN apk update

RUN apk add --no-cache \
	openssh \
	bash \
	bash-doc \
	bash-completion \
	tzdata

# 设置北京时区
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN apk del tzdata

# vi /etc/passwd
# root:x:0:0:root:/root:/bin/ash   //ash改为bash即可
