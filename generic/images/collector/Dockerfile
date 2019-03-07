FROM debian:stable

RUN apt-get -q -y update && \
    apt-get install --no-install-recommends -y -q \
    procps \
    curl \
    less \
    dnsutils \
    netcat \
    vim

RUN mkdir -p /opt/logrange/collector

COPY ["collector", "/opt/logrange/collector/"]

ENTRYPOINT ["/opt/logrange/collector/collector", "start", \
"--config-file", "/opt/logrange/collector/config.json", \
"--log-config-file", "/opt/logrange/collector/log4g.properties"]
