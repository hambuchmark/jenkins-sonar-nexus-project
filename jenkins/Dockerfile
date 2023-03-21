FROM jenkins/jenkins:lts
USER root
RUN apt-get update && \
    apt-get install -y iputils-ping && \
    apt-get install apache2 -y
CMD ["/usr/local/bin/jenkins.sh"]