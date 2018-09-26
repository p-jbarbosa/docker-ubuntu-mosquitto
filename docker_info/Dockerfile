FROM ubuntu:16.04

LABEL maintainer="joao.barbosa@hitachivantara.com"

RUN apt-get update && apt-get install -y mosquitto mosquitto-clients && \
    apt-get install -y net-tools && \
    apt-get install -y nano && \
	mkdir -p /mqtt/config /mqtt/data /mqtt/log

ADD start-mosquitto.sh /mqtt
COPY mosquitto-conf /mqtt/config
RUN chmod +x /mqtt/start-mosquitto.sh && \
    chown -R mosquitto.mosquitto /mqtt

EXPOSE 1883

CMD ["/mqtt/start-mosquitto.sh"]

# ENTRYPOINT ["bash"]