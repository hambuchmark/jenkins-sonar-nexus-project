FROM jenkins/jenkins:lts
USER root
RUN apt-get update && \
    apt-get install -y iputils-ping
CMD ["/usr/local/bin/jenkins.sh"]