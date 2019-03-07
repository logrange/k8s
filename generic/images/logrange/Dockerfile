FROM debian:stable

RUN apt-get -q -y update && \
    apt-get install --no-install-recommends -y -q \
    procps \
    curl \
    less \
    dnsutils \
    netcat \
    vim

RUN mkdir -p /opt/logrange

COPY ["logrange", "/opt/logrange/"]

ENTRYPOINT ["/opt/logrange/logrange", "start", \
"--config-file", "/opt/logrange/config.json", \
"--log-config-file", "/opt/logrange/log4g.properties"]
