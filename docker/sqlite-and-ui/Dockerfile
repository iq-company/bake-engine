ARG IMAGE_BAKER_CONDUCTOR=conductor:latest
ARG IMAGE_BAKER_UI=ui:latest

FROM ${IMAGE_BAKER_CONDUCTOR} AS conductor
FROM ${IMAGE_BAKER_UI} AS ui

# ===========================================================================================================
# 2. Bin stage
# ===========================================================================================================
FROM alpine:3.19

LABEL maintainer="service@get-iq.com"

RUN apk add openjdk17
RUN apk add nginx
RUN apk add curl

# Make app folders
RUN mkdir -p /app/config /app/logs /app/libs /app/sqlite/

# Copy the compiled output to new image
COPY ./config /app/config

# ENV CONFIG_PROP=config-sqlite.properties

COPY --from=conductor /src/conductor/docker/server/bin /app
COPY --from=conductor /src/conductor/server/build/libs/*boot*.jar /app/libs/conductor-server.jar

# Copy compiled UI assets to nginx www directory
WORKDIR /usr/share/nginx/html
RUN rm -rf ./*
COPY --from=ui /conductor/ui/build .
COPY --from=ui /conductor/docker/server/nginx/nginx.conf /etc/nginx/http.d/default.conf

# Copy the files for the server into the app folders
RUN chmod +x /app/startup.sh

HEALTHCHECK --interval=60s --timeout=30s --retries=10 CMD curl -I -XGET http://localhost:8080/health || exit 1

CMD [ "/app/startup.sh" ]
ENTRYPOINT [ "/bin/sh"]

