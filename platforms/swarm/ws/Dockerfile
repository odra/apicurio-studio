FROM centos:7
LABEL authors="Eric Wittmann <eric.wittmann@redhat.com>"
ENV RELEASE_PATH target/apicurio-studio-ws-swarm.jar
RUN yum install -y \
    java-1.8.0-openjdk-headless \
  && yum clean all

WORKDIR /opt/apicurio

ADD ${RELEASE_PATH} /opt/apicurio

RUN groupadd -r apicurio -g 1001 \
    && useradd -u 1001 -r -g apicurio -d /opt/apicurio/ -s /sbin/nologin -c "Docker image user" apicurio \
    && chown -R apicurio:apicurio /opt/apicurio/ \
    && chgrp -R 0 /opt/apicurio && chmod -R g=u /opt/apicurio

USER 1001


EXPOSE 8080


ENV APICURIO_PORT_OFFSET=0
ENV APICURIO_DB_DRIVER_NAME=h2
ENV APICURIO_DB_CONNECTION_URL=jdbc:h2:mem:apicuriodb
ENV APICURIO_DB_USER_NAME=sa
ENV APICURIO_DB_PASSWORD=sa
ENV APICURIO_DB_INITIALIZE=true
ENV APICURIO_DB_TYPE=h2
ENV APICURIO_LOGGING_LEVEL=INFO
ENV APICURIO_MIN_HEAP=768m
ENV APICURIO_MAX_HEAP=2048m


CMD java -jar /opt/apicurio/apicurio-studio-ws-swarm.jar \
    -Xms${APICURIO_MIN_HEAP} \
    -Xmx${APICURIO_MAX_HEAP} \
    -Dswarm.port.offset=${APICURIO_PORT_OFFSET} \
    -Dswarm.datasources.data-sources.ApicurioDS.driver-name=${APICURIO_DB_DRIVER_NAME} \
    -Dswarm.datasources.data-sources.ApicurioDS.connection-url=${APICURIO_DB_CONNECTION_URL} \
    -Dswarm.datasources.data-sources.ApicurioDS.user-name=${APICURIO_DB_USER_NAME} \
    -Dswarm.datasources.data-sources.ApicurioDS.password=${APICURIO_DB_PASSWORD} \
    -Dapicurio.hub.storage.jdbc.init=${APICURIO_DB_INITIALIZE} \
    -Dapicurio.hub.storage.jdbc.type=${APICURIO_DB_TYPE} \
    -Dswarm.logging=${APICURIO_LOGGING_LEVEL}
