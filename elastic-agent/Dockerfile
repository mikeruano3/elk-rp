ARG URL
ARG FLEET_SERVER_ES
ARG FLEET_SERVER_SERVICE_TOKEN
ARG FLEET_SERVER_POLICY
ARG ELK_CERTS_PATH

FROM ubuntu:latest

RUN apt-get update
RUN apt-get install -y wget curl

RUN wget https://artifacts.elastic.co/downloads/beats/elastic-agent/elastic-agent-7.17.0-linux-x86_64.tar.gz
RUN tar xzf elastic-agent-7.17.0-linux-x86_64.tar.gz
RUN mv elastic-agent-7.17.0-linux-x86_64 /elastic-agent

CMD ["sh", "-c", "./elastic-agent/elastic-agent install --url=${URL} \
  --fleet-server-es=${FLEET_SERVER_ES} \
  --fleet-server-service-token=$FLEET_SERVER_SERVICE_TOKEN \
  --fleet-server-policy=${FLEET_SERVER_POLICY} \
  --certificate-authorities=${ELK_CERTS_PATH}/ca/ca.crt \
  --fleet-server-es-ca=${ELK_CERTS_PATH}/ca/ca.crt \
  --fleet-server-cert=${ELK_CERTS_PATH}/elk-server-certs/elk-server-certs.crt \
  --fleet-server-cert-key=${ELK_CERTS_PATH}/elk-server-certs/elk-server-certs.key \
  --force && tail -f /dev/null"]
