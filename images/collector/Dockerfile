FROM debian:stretch

RUN mkdir -p /opt/logrange/lr

COPY ["lr", "/opt/logrange/lr/"]

ENTRYPOINT ["/opt/logrange/lr/lr", "collect", \
"--config-file", "/opt/logrange/lr/config/collect.json", \
"--log-config-file", "/opt/logrange/lr/config/log4g.properties"]
