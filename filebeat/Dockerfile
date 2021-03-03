FROM photon:3.0
RUN tdnf install -y shadow gawk \
    && tdnf clean all;
# Copy filebeat 
ENV PATH=/usr/share/filebeat:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
COPY --from=docker.elastic.co/beats/filebeat-oss:6.7.0 /usr/share/filebeat/ /usr/share/filebeat/
COPY --from=docker.elastic.co/beats/filebeat-oss:6.7.0 /usr/local/bin/docker-entrypoint /usr/local/bin/docker-entrypoint
RUN groupadd --gid 1000 filebeat \
    && chmod 755 /usr/local/bin/docker-entrypoint \
    && mkdir -p /usr/share/filebeat/data /usr/share/filebeat/logs \
    && chown -R root:filebeat /usr/share/filebeat \
    && find /usr/share/filebeat -type d -exec chmod 0750 {} \; \
    && find /usr/share/filebeat -type f -exec chmod 0640 {} \; \
    && chmod 0750 /usr/share/filebeat/filebeat \ 
    && chmod 0770 /usr/share/filebeat/modules.d \
    && chmod 0770 /usr/share/filebeat/data /usr/share/filebeat/logs \
    && useradd -M --uid 1000 --gid 1000 --home /usr/share/filebeat filebeat
USER filebeat
WORKDIR /usr/share/filebeat
ENTRYPOINT ["/usr/local/bin/docker-entrypoint"]
CMD ["-e"]