FROM sonarqube
USER root
RUN apt-get update && \
    apt-get install -y iputils-ping
CMD ["/opt/sonarqube/bin/run.sh"]