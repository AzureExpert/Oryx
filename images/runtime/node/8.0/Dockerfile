# Startup script generator
FROM golang:1.11-stretch as startupCmdGen
WORKDIR /go/src/startupscriptgenerator/
COPY src/startupscriptgenerator .
ARG GIT_COMMIT=unspecified
ARG BUILD_NUMBER=unspecified
ENV GIT_COMMIT=${GIT_COMMIT}
ENV BUILD_NUMBER=${BUILD_NUMBER}
RUN ./build.sh node /opt/startupcmdgen/startupcmdgen

FROM mcr.microsoft.com/oryx/node-8.0-base:20190518.7

# Bake Application Insights key from pipeline variable into final image
ARG AI_KEY
ENV ORYX_AI_INSTRUMENTATION_KEY=${AI_KEY}

COPY --from=startupCmdGen /opt/startupcmdgen/startupcmdgen /opt/startupcmdgen/startupcmdgen
RUN ln -s /opt/startupcmdgen/startupcmdgen /usr/local/bin/oryx

# Node wrapper is used to debug apps when node is executed indirectly, e.g. by npm.
COPY src/startupscriptgenerator/node/wrapper/node /opt/node-wrapper/
RUN chmod a+x /opt/node-wrapper/node