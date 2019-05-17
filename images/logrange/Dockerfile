FROM debian:stretch

RUN mkdir -p /opt/logrange

COPY ["logrange", "/opt/logrange/"]

ENTRYPOINT ["/opt/logrange/logrange", "start", \
"--config-file", "/opt/logrange/config/logrange.json", \
"--log-config-file", "/opt/logrange/config/log4g.properties"]
