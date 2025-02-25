FROM docker.io/bitnami/minideb:bullseye
ENV HOME="/" \
    OS_ARCH="amd64" \
    OS_FLAVOUR="debian-11" \
    OS_NAME="linux"

LABEL org.opencontainers.image.authors="https://bitnami.com/contact" \
      org.opencontainers.image.description="Application packaged by Bitnami" \
      org.opencontainers.image.ref.name="0.14.0-debian-11-r32" \
      org.opencontainers.image.source="https://github.com/bitnami/containers/tree/main/bitnami/mysqld-exporter" \
      org.opencontainers.image.title="mysqld-exporter" \
      org.opencontainers.image.vendor="VMware, Inc." \
      org.opencontainers.image.version="0.14.0"

COPY prebuildfs /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN install_packages ca-certificates curl gzip procps tar
RUN mkdir -p /tmp/bitnami/pkg/cache/ && cd /tmp/bitnami/pkg/cache/ && \
    if [ ! -f mysqld-exporter-0.14.0-154-linux-amd64-debian-11.tar.gz ]; then \
      curl -SsLf https://downloads.bitnami.com/files/stacksmith/mysqld-exporter-0.14.0-154-linux-amd64-debian-11.tar.gz -O ; \
    fi && \
    echo "d96f14069681e981064a89f1f6d08ca9bd4bc01eee1e1496ec2106c7d4b94e1d  mysqld-exporter-0.14.0-154-linux-amd64-debian-11.tar.gz" | sha256sum -c - && \
    tar -zxf mysqld-exporter-0.14.0-154-linux-amd64-debian-11.tar.gz -C /opt/bitnami --strip-components=2 --no-same-owner --wildcards '*/files' && \
    rm -rf mysqld-exporter-0.14.0-154-linux-amd64-debian-11.tar.gz
RUN apt-get update && apt-get upgrade -y && \
    rm -r /var/lib/apt/lists /var/cache/apt/archives
RUN chmod g+rwX /opt/bitnami
RUN ln -sf /opt/bitnami/mysqld-exporter/bin/mysqld_exporter /bin/mysqld_exporter

ENV APP_VERSION="0.14.0" \
    BITNAMI_APP_NAME="mysqld-exporter" \
    PATH="/opt/bitnami/mysqld-exporter/bin:$PATH"

EXPOSE 9104

WORKDIR /opt/bitnami/mysqld-exporter
USER 1001
ENTRYPOINT [ "mysqld_exporter" ]
