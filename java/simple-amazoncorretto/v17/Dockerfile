FROM amazoncorretto:17.0.4-alpine as corretto-jdk

# required for strip-debug to work
RUN apk add --no-cache binutils

# Build small JRE image
RUN $JAVA_HOME/bin/jlink \
         --verbose \
         --add-modules ALL-MODULE-PATH \
         --strip-debug \
         --no-man-pages \
         --no-header-files \
         --compress=2 \
         --output /jre

FROM alpine:latest
ENV JAVA_HOME=/jre
ENV PATH="${JAVA_HOME}/bin:${PATH}"

# copy JRE from the base image
COPY --from=corretto-jdk /jre $JAVA_HOME

# Add app user
ARG APPLICATION_USER=app
RUN adduser -u 1000 -D $APPLICATION_USER

# Configure working directory
RUN mkdir /app
RUN chown -R $APPLICATION_USER /app

USER 1000
