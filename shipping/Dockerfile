FROM debian:10 AS build
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y maven
WORKDIR /opt/shipping
COPY pom.xml /opt/shipping/
RUN mvn dependency:resolve
COPY src /opt/shipping/src/
RUN mvn package
# Install missing dependencies explicitly
RUN apt-get install -y --fix-missing ca-certificates-java openjdk-11-jre-headless:amd64

# Clean up cached package files
RUN apt-get clean && apt-get autoclean

# Retry the failed steps
RUN mvn packag

FROM openjdk:8-jre-alpine
EXPOSE 8080
WORKDIR /opt/shipping
ENV CART_ENDPOINT=cart:8080
ENV DB_HOST=mysql
COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar
CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]
# #
# # Build
# #
# FROM debian:10 AS build
# RUN apt-get update && apt-get -y install maven
# WORKDIR /opt/shipping
# COPY pom.xml /opt/shipping/
# RUN mvn dependency:resolve
# COPY src /opt/shipping/src/
# RUN mvn package

# #
# # Run
# #
# FROM openjdk:8-jdk
# EXPOSE 8080
# WORKDIR /opt/shipping
# ENV CART_ENDPOINT=cart:8080
# ENV DB_HOST=mysql
# COPY --from=build /opt/shipping/target/shipping-1.0.jar shipping.jar
# CMD [ "java", "-Xmn256m", "-Xmx768m", "-jar", "shipping.jar" ]

