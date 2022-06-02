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
RUN cd /usr/share/elasticsearch && \
	echo 'Y' | bin/elasticsearch-certutil ca --pem --out /elkcerts/certs/elastic-stack-ca.zip --days 3650

# unzip CA certs
RUN cd /elkcerts/certs/ && unzip elastic-stack-ca.zip

# Generate .crt and .key for elasticsearch
RUN cd /usr/share/elasticsearch && \
	echo 'Y' | bin/elasticsearch-certutil cert \
	--in /ssl/instances.yml \
	--ca-cert /elkcerts/certs/ca/ca.crt \
	--ca-key /elkcerts/certs/ca/ca.key \
	--days 3650 \
	--out /elkcerts/certs/certificate-bundle.zip \
	--pem

# Unzip CERT bundle
RUN cd /elkcerts/certs && unzip certificate-bundle.zip 

# Generate logstash pkcs8 key
RUN openssl pkcs8 -in /elkcerts/certs/logstash/logstash.key -topk8 -nocrypt -out /elkcerts/certs/logstash/logstash.pkcs8.key
RUN chmod --reference=/elkcerts/certs/logstash/logstash.key /elkcerts/certs/logstash/logstash.pkcs8.key

# Copy key to usr/share
RUN cp -r /elkcerts/certs /usr/share/elasticsearch/config

USER elasticsearch:root
