FROM photon:3.0
RUN tdnf install -y ca-certificates shadow libpcap wget unzip gawk \
    && tdnf clean all;
RUN groupadd vault && useradd -r -m -G vault vault
ENV VAULT_VERSION 0.10.1
ENV ARCH amd64
ENV PATH /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
# Skip setting IPC_LOCK in entrypoint script
ENV SKIP_SETCAP true
COPY --from=vault:0.10.1 /usr/local/bin/docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
COPY --from=vault:0.10.1 /usr/bin/dumb-init /usr/bin/dumb-init
RUN set -eux \
    && mkdir -p /tmp/build \
    && cd /tmp/build \ 
    && wget https://releases.hashicorp.com/vault/${VAULT_VERSION}/vault_${VAULT_VERSION}_linux_${ARCH}.zip \
    && unzip -d /bin vault_${VAULT_VERSION}_linux_${ARCH}.zip \
    && chmod 755 /bin/vault \
    && cd /tmp && rm -rf /tmp/build \
    && mkdir -p /vault/logs \
    && mkdir -p /vault/file \
    && mkdir -p /vault/config \
    && chown -R vault:vault /vault \
    # Set IPC lock for vault binary
    # Requires running the container with --cap-add=IPC_LOCK or
    # securityContext:
    #   capabilities:
    #     add:
    #     - IPC_LOCK
    && setcap cap_ipc_lock=+ep $(readlink -f $(which vault)) \
    # Always running as vault user, remove su-exec from script
    && sed -i 's/su-exec//g' /usr/local/bin/docker-entrypoint.sh
VOLUME "/vault/logs"
VOLUME "/vault/file"
EXPOSE 8200/tcp
USER vault
ENTRYPOINT ["/usr/local/bin/docker-entrypoint.sh"]
CMD ["server", "-dev"]