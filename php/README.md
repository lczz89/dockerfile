docker build --build-arg PHP_VERSION=7.1.30 -t php7.1.30 .
docker tag d7c876402445  registry.cn-hangzhou.aliyuncs.com/zev-php/php:7.1.30
docker push registry.cn-hangzhou.aliyuncs.com/zev-php/php:7.1.30

#5.6.40
docker build  -t php5.6.40 .
docker tag d7c876402445  registry.cn-hangzhou.aliyuncs.com/zev-php/php:5.6.40
docker push registry.cn-hangzhou.aliyuncs.com/zev-php/php:5.6.40