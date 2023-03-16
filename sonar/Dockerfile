FROM sonarqube
USER root
RUN apt-get update && \
    apt-get install -y iputils-ping
COPY sonar.properties /opt/sonarqube/conf/