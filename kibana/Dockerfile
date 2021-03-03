FROM photon:3.0
EXPOSE 5601

# Add Reporting dependencies.
RUN tdnf install -y shadow coreutils fontconfig freetype2 kibana \
    && tdnf clean all;

WORKDIR /usr/share/kibana
#RUN ln -s /usr/share/kibana /opt/kibana

ENV ELASTIC_CONTAINER true
ENV PATH=/usr/share/kibana/bin:$PATH

# Set some Kibana configuration defaults.
COPY --chown=kibana:0 config/kibana.yml /etc/kibana/kibana.yml
COPY --chown=kibana:0 bin/kibana-docker /usr/local/bin/

# Ensure gid 0 write permissions for OpenShift.
RUN chmod -R g+ws /usr/share/kibana 

USER kibana

LABEL org.label-schema.schema-version="1.0" org.label-schema.vendor="Elastic" org.label-schema.name="kibana" org.label-schema.version="6.8.14" org.label-schema.url="https://www.elastic.co/products/kibana" org.label-schema.vcs-url="https://github.com/elastic/kibana" org.label-schema.license="Elastic License" license="Elastic License"

CMD ["/usr/local/bin/kibana-docker"]