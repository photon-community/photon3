FROM photon:3.0
RUN tdnf install -y ca-certificates shadow libpcap wget unzip gawk \
    && tdnf clean all;
RUN groupadd vault && useradd -r -m -G vault vault
ENV VAULT_VERSION 0.10.1
ENV ARCH amd64
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
COPY --from=vault:0.10.1 /usr/local/bin/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
COPY --from=vault:0.10.1 /usr/bin/dumb-init /usr/bin/dumb-init
COPY --from=vault:0.10.1 /sbin/su-exec /sbin/su-exec
RUN set -eux \
    && mkdir -p /tmp/build \
    && cd /tmp/build \ 
    && wget https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_${ARCH}.zip \
    && wget https://github.com/songdongsheng/su-exec/releases/download/1.3/su-exec-glibc-static \
    && mv su-exec-glibc-static /sbin/su-exec \
    && chmod 755 /sbin/su-exec \
    && unzip -d /bin vault_${VAULT_VERSION}_linux_${ARCH}.zip \
    && chmod 755 /bin/vault \
    && cd /tmp && rm -rf /tmp/build \
    && mkdir -p /vault/logs \
    && mkdir -p /vault/file \
    && mkdir -p /vault/config \
    && chown -R vault:vault /vault 
VOLUME "/vault/logs"
VOLUME "/vault/file"
EXPOSE 8200/tcp
WORKDIR /root
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
CMD ["server", "-dev"]