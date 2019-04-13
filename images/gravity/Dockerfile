FROM debian:stable

RUN apt-get -q -y update && \
    apt-get install --no-install-recommends -y -q \
    procps \
    curl \
    less \
    dnsutils \
    netcat \
    vim

RUN mkdir -p /opt/logrange/gravity

COPY ["gravity", "/opt/logrange/gravity/"]

ENTRYPOINT ["/opt/logrange/gravity/gravity", "start", \
"--config-file", "/opt/logrange/gravity/config/adapter.json", \
"--log-config-file", "/opt/logrange/gravity/config/log4g.properties"]
