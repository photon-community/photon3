FROM photon:3.0
RUN tdnf install -y shadow gawk \
    && tdnf clean all;
# Copy beat-exporter 
COPY --from=trustpilot/beat-exporter:0.1.1 /bin/beat-exporter /bin/beat-exporter 
RUN groupadd --gid 1000 filebeat \
    && chmod 755 /bin/beat-exporter \
    && chown -R root:filebeat /bin/beat-exporter \
    && useradd -M --uid 1000 --gid 1000 --home /usr/share/filebeat filebeat
USER filebeat
EXPOSE 9479
ENTRYPOINT ["/bin/beat-exporter"]
