FROM debian:latest AS build

# Set the TERM environment variable
ENV TERM=xterm

# Update package lists
RUN apt-get update

# Install apt-utils
RUN apt-get install -y apt-utils

# Reconfigure ca-certificates-java
RUN dpkg --configure -a

# Fix unmet dependencies
RUN apt-get -f install -y

# Remove and reinstall problematic packages
RUN apt-get remove --purge -y ca-certificates-java openjdk-11-jre-headless:amd64
RUN apt-get install -y ca-certificates-java openjdk-11-jre-headless:amd64

# Install Maven
RUN apt-get install -y maven

WORKDIR /opt/shipping

# Copy the project's POM file to the container
COPY pom.xml /opt/shipping/

# Resolve Maven dependencies
RUN mvn dependency:resolve

# Copy the source code to the container
COPY src /opt/shipping/src/

# Package the application
RUN mvn package






# FROM debian:10 AS build
# RUN apt-get update && apt-get -y install maven
# RUN apt-get install -y apt-utils
# WORKDIR /opt/shipping
# COPY pom.xml /opt/shipping/
# RUN mvn dependency:resolve
# COPY src /opt/shipping/src/
# RUN mvn package

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