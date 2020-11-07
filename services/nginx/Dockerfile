ARG NGINX_VERSION
FROM ${NGINX_VERSION}

ARG MAINTAINER

MAINTAINER  ${MAINTAINER}

#  修改镜像源
RUN if [ $CONTAINER_PACKAGE_URL ] ; then sed -i "s/dl-cdn.alpinelinux.org/${CONTAINER_PACKAGE_URL}/g" /etc/apk/repositories ; fi