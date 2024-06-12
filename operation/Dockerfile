FROM alpine:latest
COPY authorized_keys /root/.ssh/authorized_keys
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories && \
    set -eux; \
	apk update && \
	apk add --no-cache openssh openssh-server mysql-client
RUN chmod 600 ~/.ssh/authorized_keys
RUN cd /etc/ssh && ssh-keygen -A
RUN sed -i "s/#Port.*/Port 22/g" /etc/ssh/sshd_config
EXPOSE 22
CMD /usr/sbin/sshd

# docker run -dit -p 4524:22 alpinesshd sh
# 后台运行之后，进入手动启动 /usr/sbin/sshd 
#ssh  -p4524 root@8.214.45.24 即可远程连接
#mysql -hrm-t4nn78hzy2k91yf0n.mysql.singapore.rds.aliyuncs.com -ufinno_tester -p'OugyjN4$(Fnmfr' helios_pro