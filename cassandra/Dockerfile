
FROM photon:3.0

RUN set -eux; \
	tdnf install -y openjdk8 cassandra \
	shadow numactl iproute2 \
	python2 python-cqlsh coreutils util-linux \
	&& tdnf clean all;

ENV JAVA_HOME="/lib/jvm/jre"
ENV CASSANDRA_HOME /var/opt/cassandra
ENV CASSANDRA_CONF /etc/cassandra
ENV PATH $CASSANDRA_HOME/bin:$PATH

ENV CASSANDRA_VERSION 3.11.10

VOLUME /var/lib/cassandra
RUN mkdir -p "$CASSANDRA_CONF" /var/lib/cassandra /var/log/cassandra; \
	chown -R cassandra:cassandra "$CASSANDRA_CONF" /var/lib/cassandra /var/log/cassandra; \
	chmod 777 "$CASSANDRA_CONF" /var/lib/cassandra /var/log/cassandra; 

COPY docker-entrypoint.sh /usr/local/bin/
RUN ln -s usr/local/bin/docker-entrypoint.sh /docker-entrypoint.sh # backwards compat
ENTRYPOINT ["docker-entrypoint.sh"]

# 7000: intra-node communication
# 7001: TLS intra-node communication
# 7199: JMX
# 9042: CQL
# 9160: thrift service
EXPOSE 7000 7001 7199 9042 9160
USER cassandra
CMD ["cassandra", "-f"]
