ARG ELASTIC_VERSION

# https://www.docker.elastic.co/
FROM docker.elastic.co/beats/elastic-agent:${ELASTIC_VERSION}

# Add your kibana plugins setup here
# Example: RUN kibana-plugin install <name|url>
USER root
COPY . /

RUN cp /config/filebeat.yml /usr/share/elastic-agent/data/elastic-agent-e38418/install/filebeat-8.2.2-linux-x86_64/filebeat.yml
WORKDIR /usr/share/elastic-agent/data/elastic-agent-e38418/install/filebeat-8.2.2-linux-x86_64
RUN chown -R root:root filebeat.yml
RUN chown -R root:root module/azure*

RUN ./filebeat modules enable azure
RUN chown -R root:root modules.d/azure.yml
RUN cp /config/modules/azure.yml /config/modules/azure1.yml
RUN mv /config/modules/azure1.yml modules.d/azure.yml

RUN cp /config/metricbeat.yml /usr/share/elastic-agent/data/elastic-agent-e38418/install/metricbeat-8.2.2-linux-x86_64/metricbeat.yml
WORKDIR /usr/share/elastic-agent/data/elastic-agent-e38418/install/metricbeat-8.2.2-linux-x86_64
RUN chown -R root:root metricbeat.yml
RUN chown -R root:root module/azure*

RUN ./metricbeat modules enable azure
RUN chown -R root:root modules.d/azure.yml
RUN cp /config/modules/azure.yml /config/modules/azure1.yml
RUN mv /config/modules/azure1.yml modules.d/azure.yml

WORKDIR /usr/share/elastic-agent/

USER elastic-agent