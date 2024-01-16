ARG STACK_VERSION

FROM docker.elastic.co/beats/filebeat:${STACK_VERSION}

COPY filebeat.yml /usr/share/filebeat/filebeat.yml
COPY mysql.yml /etc/filebeat/modules.d/mysql.yml

RUN filebeat modules enable mysql