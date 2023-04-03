FROM jenkins/jenkins:jdk11
USER root
RUN apt-get update && \
    apt-get install -y iputils-ping && \
    apt-get install apache2 -y && \
    apt-get install wget -y && \
    apt-get install nano -y
RUN cd /opt && \
    wget https://downloads.apache.org/maven/maven-3/3.9.1/binaries/apache-maven-3.9.1-bin.tar.gz && \
    tar -xf apache-maven-3.9.1-bin.tar.gz && \
    rm apache-maven-3.9.1-bin.tar.gz && \
    ln -s /opt/apache-maven-3.9.1/bin/mvn /usr/local/bin/mvn
RUN curl -sL https://deb.nodesource.com/setup_14.x | bash - \
    && apt-get install -y nodejs 
CMD ["/usr/local/bin/jenkins.sh"]