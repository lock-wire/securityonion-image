FROM openjdk:8u212-jre-alpine

LABEL org.label-schema.name="kafka" \
      org.label-schema.description="Apache Kafka" \
      org.label-schema.build-date="${build_date}" \
      org.label-schema.vcs-url="https://github.com/wurstmeister/kafka-docker" \
      org.label-schema.vcs-ref="${vcs_ref}" \
      org.label-schema.version="${scala_version}_${kafka_version}" \
      org.label-schema.schema-version="1.0" \
      maintainer="wurstmeister"

ENV KAFKA_VERSION=2.7.0 \
    SCALA_VERSION=2.13 \
    KAFKA_HOME=/opt/kafka \
    GLIBC_VERSION=2.31-r0 \
    FILENAME="kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz" 

ENV PATH=${PATH}:${KAFKA_HOME}/bin

RUN apk add --no-cache bash curl jq 

RUN url=$(curl --stderr /dev/null "https://www.apache.org/dyn/closer.cgi?path=/kafka/${KAFKA_VERSION}/${FILENAME}&as_json=1" | jq -r '"\(.preferred)\(.path_info)"') \
echo "Downloading Kafka from $url" \
&& wget "${url}" -O "/tmp/${FILENAME}"

RUN tar xfz /tmp/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz -C /opt \
&& rm /tmp/kafka_${SCALA_VERSION}-${KAFKA_VERSION}.tgz \
&& ln -s /opt/kafka_${SCALA_VERSION}-${KAFKA_VERSION} ${KAFKA_HOME} \
&& rm /tmp/* \
&& wget https://github.com/sgerrand/alpine-pkg-glibc/releases/download/${GLIBC_VERSION}/glibc-${GLIBC_VERSION}.apk \
&& apk add --no-cache --allow-untrusted glibc-${GLIBC_VERSION}.apk \
&& rm glibc-${GLIBC_VERSION}.apk

# Use "exec" form so that it runs as PID 1 (useful for graceful shutdown)
#CMD ["start-kafka.sh"]