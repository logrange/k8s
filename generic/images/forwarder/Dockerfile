FROM debian:stable

RUN apt-get -q -y update && \
    apt-get install --no-install-recommends -y -q \
    procps \
    curl \
    less \
    dnsutils \
    netcat \
    vim

RUN mkdir -p /opt/logrange/lr

COPY ["lr", "/opt/logrange/lr/"]

ENTRYPOINT ["/opt/logrange/lr/lr", "forward", \
"--config-file", "/opt/logrange/lr/forward.json", \
"--log-config-file", "/opt/logrange/lr/log4g.properties"]
