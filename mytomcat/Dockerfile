# Dockerfile for s2i builder image
# Ref. docs 
# http://tecadmin.net/steps-to-install-tomcat-server-on-centos-rhel/#
# http://tecadmin.net/steps-to-install-java-on-centos-5-6-or-rhel-5-6/

FROM centos:7

MAINTAINER sbylo@redhat.com

ENV TOMCAT_VERSION=7.0.70

# Install java
RUN cd /opt && yum -y install tar wget && wget -O - -q --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz" | tar xzf - && yum clean all -y

# Install Tomcat
Run cd /usr/local && curl -s http://www.us.apache.org/dist/tomcat/tomcat-7/v7.0.70/bin/apache-tomcat-7.0.70.tar.gz | tar xzf - && mv apache-tomcat-7.0.70 tomcat7

RUN useradd -d /home/tomcat -m -u 1001 -g 0 tomcat && cd /usr/local/tomcat7 && chown -R tomcat:0 . 

WORKDIR /usr/local/tomcat7

ENV JAVA_HOME=/opt/jdk1.7.0_79 JRE_HOME=/opt/jdk1.7.0_79/jre 

EXPOSE 8080

USER 1001

ENTRYPOINT ./bin/startup.sh && sleep 9999999999

