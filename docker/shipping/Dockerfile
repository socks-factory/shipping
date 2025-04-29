FROM maven:3.8-jdk-8 AS build

ARG BUILD_DATE=""
ARG BUILD_VERSION="0.0.2-SNAPSHOT"
ARG COMMIT="test"

ENV BUILD_VERSION=${BUILD_VERSION}
ENV BUILD_DATE=${BUILD_DATE}
ENV COMMIT=${COMMIT}

COPY . /usr/src/mymaven
WORKDIR /usr/src/mymaven

RUN mvn -DskipTests package

FROM openjdk:8-jre-alpine

WORKDIR /usr/src/app
COPY --from=build /usr/src/mymaven/target/shipping.jar ./app.jar

RUN	chown -R myuser:mygroup ./app.jar

USER myuser

ARG BUILD_DATE=""
ARG BUILD_VERSION="0.0.2-SNAPSHOT"
ARG COMMIT="test"

ENV BUILD_VERSION=${BUILD_VERSION}
ENV BUILD_DATE=${BUILD_DATE}
ENV COMMIT=${COMMIT}

LABEL org.label-schema.vendor="SUSE" \
  org.label-schema.build-date="${BUILD_DATE}" \
  org.label-schema.version="${BUILD_VERSION}" \
  org.label-schema.name="Socks Shop: Shipping" \
  org.label-schema.description="REST API for Shipping service" \
  org.label-schema.url="https://github.com/microservices-demo/shipping" \
  org.label-schema.vcs-url="github.com:microservices-demo/shipping.git" \
  org.label-schema.vcs-ref="${COMMIT}" \
  org.label-schema.schema-version="1.0"

ENTRYPOINT ["java","-jar","./app.jar", "--port=80"]
