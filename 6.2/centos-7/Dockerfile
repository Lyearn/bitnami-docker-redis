FROM centos:7
LABEL maintainer "Bitnami <containers@bitnami.com>"

ENV HOME="/" \
    OS_ARCH="x86_64" \
    OS_FLAVOUR="centos-7" \
    OS_NAME="linux"

COPY prebuildfs /

RUN yum install -y epel-release

# Install required system packages and dependencies
RUN yum install -y acl ca-certificates curl gzip glibc openssl procps tar
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "wait-for-port" "1.0.1-5" --checksum 488628d237cb319add76d0e3989d18990562db323949d9968f5c2588a3ea04d1
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "redis" "6.2.7-3" --checksum 867845f3e1eb3928871b94dd5ee74995280e1f978dcef57606b2d06ca74f4025
RUN . /opt/bitnami/scripts/libcomponent.sh && component_unpack "gosu" "1.14.0-1" --checksum 5d374042b9320f3c931294d854835ffe602b499b2d71a4274769e12e6ba37eb7
RUN yum update -y && yum upgrade -y && \
    yum clean all && rm -fR /var/cache/yum
RUN chmod g+rwX /opt/bitnami
RUN ln -s /opt/bitnami/scripts/redis/entrypoint.sh /entrypoint.sh
RUN ln -s /opt/bitnami/scripts/redis/run.sh /run.sh

COPY rootfs /
RUN /opt/bitnami/scripts/redis/postunpack.sh
ENV BITNAMI_APP_NAME="redis" \
    BITNAMI_IMAGE_VERSION="6.2.7-3-centos-7" \
    PATH="/opt/bitnami/common/bin:/opt/bitnami/redis/bin:$PATH"

EXPOSE 6379

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/redis/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/redis/run.sh" ]
