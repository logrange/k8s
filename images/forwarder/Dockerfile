FROM debian:stretch

RUN mkdir -p /opt/logrange/lr

COPY ["lr", "/opt/logrange/lr/"]

ENTRYPOINT ["/opt/logrange/lr/lr", "forward", \
"--config-file", "/opt/logrange/lr/config/forward.json", \
"--log-config-file", "/opt/logrange/lr/config/log4g.properties"]
