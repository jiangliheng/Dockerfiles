# AdoptOpenjdk-centos-jre-hotspot-8u292-b10
# 基于 AdoptOpenJDK 官方修改：https://github.com/AdoptOpenJDK/openjdk-docker/blob/master/8/jre/
# 修改点：
# 1. 修改字符集为： zh_CN.UTF-8
# 2. 修改时区为：Asia/Shanghai

# 基础镜像
FROM centos:7

# 作者信息
MAINTAINER jiang_liheng <jiang_liheng@163.com>

# 字符集变量
ENV LANG='zh_CN.UTF-8'

# 时区变量
ENV TIME_ZONE Asia/Shanghai

# 安装依赖，设置时区
RUN yum install -y tzdata openssl curl ca-certificates fontconfig gzip tar; \
    yum update -y; yum clean all; \
    cp /usr/share/zoneinfo/${TIME_ZONE} /etc/localtime; \
    echo "${TIME_ZONE}" > /etc/timezone; \
    echo "LANG=zh_CN.UTF-8" > /etc/locale.conf;

# java 版本
ENV JAVA_VERSION jdk8u292-b10

# 安装 jre
RUN set -eux; \
    ARCH="$(uname -m)"; \
    case "${ARCH}" in \
       aarch64|arm64) \
         ESUM='b062ec775e6c2961532d9afeae4027fe3ac2cf4344cbc912a401be5bfb6ca221'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jre_aarch64_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       armhf|armv7l) \
         ESUM='7f7707a7a3998737d2221080ea65d50ea96f5dde5226961ebcebd3ec99a82a32'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jre_arm_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       ppc64el|ppc64le) \
         ESUM='245ecd0dfde7e763c0b65028aa0440489466926be2ba018977ac9047dc328a8e'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jre_ppc64le_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       amd64|x86_64) \
         ESUM='cad66f48f90167ed19030c71f8f0580702c43cce5ce5a0d76833f7a5ae7c402a'; \
         BINARY_URL='https://github.com/AdoptOpenJDK/openjdk8-binaries/releases/download/jdk8u292-b10/OpenJDK8U-jre_x64_linux_hotspot_8u292b10.tar.gz'; \
         ;; \
       *) \
         echo "Unsupported arch: ${ARCH}"; \
         exit 1; \
         ;; \
    esac; \
    curl -LfsSo /tmp/openjdk.tar.gz ${BINARY_URL}; \
    echo "${ESUM} */tmp/openjdk.tar.gz" | sha256sum -c -; \
    mkdir -p /opt/java/openjdk; \
    cd /opt/java/openjdk; \
    tar -xf /tmp/openjdk.tar.gz --strip-components=1; \
    rm -rf /tmp/openjdk.tar.gz;

# 设置环境变量
ENV JAVA_HOME=/opt/java/openjdk \
    PATH="/opt/java/openjdk/bin:$PATH"
