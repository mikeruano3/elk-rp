ARG ELASTIC_VERSION

# https://www.docker.elastic.co/
FROM docker.elastic.co/elasticsearch/elasticsearch:${ELASTIC_VERSION}

# Add your elasticsearch plugins setup here
# Example: RUN elasticsearch-plugin install analysis-icu

USER root
COPY . /
#RUN chmod +x /entrypoint.sh

## STEPS TO GENERATE CERTS ON VOLUME
# Create folder
RUN mkdir -p /elkcerts/certs; \
	chown elasticsearch /elkcerts*;

# Create CA certs
#RUN cd /usr/share/elasticsearch && \
#	echo 'Y' | bin/elasticsearch-certutil ca --pem --out /elkcerts/certs/elastic-stack-ca.zip --days 3650

# unzip CA certs
#RUN cd /elkcerts/certs/ && unzip elastic-stack-ca.zip

# RUN cp /ssl/ca/elk-cert.pfx /elkcerts/certs/ca/elk-cert.pfx
RUN mkdir /elkcerts/certs/ca
RUN cp /ssl/ca/ca.crt /elkcerts/certs/ca/ca.crt
RUN cp /ssl/ca/ca.key /elkcerts/certs/ca/ca.key

# Generate .crt and .key for elasticsearch
# RUN cd /usr/share/elasticsearch && \
# 	echo 'Y' | bin/elasticsearch-certutil cert \
# 	--in /ssl/instances.yml \
# 	--ca-cert /elkcerts/certs/ca/ca.crt \
# 	--ca-key /elkcerts/certs/ca/ca.key \
# 	--days 3650 \
# 	--out /elkcerts/certs/certificate-bundle.zip \
# 	--pem

# Unzip CERT bundle
# RUN cd /elkcerts/certs && unzip certificate-bundle.zip 

# Generate logstash pkcs8 key
# RUN openssl pkcs8 -in /elkcerts/certs/logstash/logstash.key -topk8 -nocrypt -out /elkcerts/certs/logstash/logstash.pkcs8.key
# RUN chmod --reference=/elkcerts/certs/logstash/logstash.key /elkcerts/certs/logstash/logstash.pkcs8.key


RUN mkdir -p /elkcerts/certs/elk-server-certs
RUN mkdir -p /elkcerts/certs/kibana
RUN mkdir -p /elkcerts/certs/logstash
RUN mkdir -p /elkcerts/certs/elasticagent
RUN mkdir -p /elkcerts/certs/fleet

RUN cp -p /ssl/ca/ca.crt /elkcerts/certs/elk-server-certs/elk-server-certs.crt
RUN cp -p /ssl/ca/ca.key /elkcerts/certs/elk-server-certs/elk-server-certs.key
RUN cp -p /ssl/ca/ca.crt /elkcerts/certs/kibana/kibana.crt
RUN cp -p /ssl/ca/ca.key /elkcerts/certs/kibana/kibana.key
RUN cp -p /ssl/ca/ca.crt /elkcerts/certs/logstash/logstash.crt
RUN cp -p /ssl/ca/ca.key /elkcerts/certs/logstash/logstash.key
RUN cp -p /ssl/ca/ca.crt /elkcerts/certs/elasticagent/elasticagent.crt
RUN cp -p /ssl/ca/ca.key /elkcerts/certs/elasticagent/elasticagent.key
RUN cp -p /ssl/ca/ca.crt /elkcerts/certs/fleet/fleet.crt
RUN cp -p /ssl/ca/ca.key /elkcerts/certs/fleet/fleet.key

RUN cp -p /ssl/elkcert.pfx /elkcerts/certs/logstash/logstash.pkcs8.key

# Copy key to usr/share
RUN cp -r /elkcerts/certs /usr/share/elasticsearch/config

USER elasticsearch:root
