FROM registry.access.redhat.com/rhel6.5

MAINTAINER sbylo@redhat.com

ENV TOMCAT_VERSION=7.0.70

# Set labels used in OpenShift to describe the builder image
LABEL io.k8s.description="Platform for serving web content" \
      io.k8s.display-name="Tomcat 7.0.70" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,html,tomcat"

# Install some tools
RUN yum -y install unzip tar wget && yum clean all -y

# Install java 7
#RUN cd /opt && \
#wget -O - -q --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u79-b15/jdk-7u79-linux-x64.tar.gz" | tar xzf - && ln -s /opt/jdk1.7.0_79/bin/jar /usr/bin/jar

# Install java 8
RUN cd /opt && \
wget -O - -q --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/8u91-b14/jdk-8u91-linux-x64.tar.gz" | tar xzf - && ln -s /opt/jdk1.8.0_91/bin/jar /usr/bin/jar

ENV JAVA_HOME=/opt/jdk1.8.0_91 JRE_HOME=/opt/jdk1.8.0_91/jre 

# Install Tomcat
RUN cd /usr/local && curl -s http://www.us.apache.org/dist/tomcat/tomcat-7/v7.0.70/bin/apache-tomcat-7.0.70.tar.gz | tar xzf - && mv apache-tomcat-7.0.70 tomcat7 && rm -rf tomcat7/webapps/*


WORKDIR /usr/local/tomcat7

EXPOSE 8080

RUN useradd -d /home/tomcat -m -u 1001 -g 0 tomcat && cd /usr/local/tomcat7 && chown -R tomcat:0 . 

# Defines the location of the S2I scripts
LABEL io.openshift.s2i.scripts-url=image:///usr/local/s2i

# Copy the S2I scripts from ./.s2i/bin/ to /usr/local/s2i when making the builder image
COPY ./.s2i/bin/ /usr/local/s2i

# BTPN: Install the baseline war file Sample.war
COPY ./Sample.war /usr/local/tomcat7/webapps/
RUN chown tomcat:0 /usr/local/tomcat7/webapps/Sample.war
USER 1001
RUN cd /usr/local/tomcat7/webapps && mkdir Sample && cd Sample && unzip ../Sample.war 

#ENTRYPOINT ./bin/startup.sh && sleep 9999999999
# Set the default CMD to print the usage of the image, if somebody does docker run
CMD ["usage"]

