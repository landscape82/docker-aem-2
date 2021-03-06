FROM centos:7
MAINTAINER Yury Bialykh

# Java
ENV JAVA_VERSION 8u102
ENV BUILD_VERSION b14

# AEM
ENV CQ_FOREGROUND y
ENV CQ_VERBOSE    y
ENV CQ_NOBROWSER  y
ENV CQ_JVM_OPTS   "-server -Xmx1536M -Xms512M -agentlib:jdwp=transport=dt_socket,address=8000,server=y,suspend=n"
ENV AEM_QUICKSTART_FILE AEM_Quickstart.jar
ENV AEM_WORKING_DIR /opt/aem

# Upgrading system
RUN yum -y upgrade && \
    yum -y install wget && \
    yum clean all
    
# Downloading Java
RUN wget --no-cookies --no-check-certificate --header "Cookie: oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/$JAVA_VERSION-$BUILD_VERSION/jre-$JAVA_VERSION-linux-x64.rpm" -O /tmp/jre-8-linux-x64.rpm

# Installing Java
RUN yum -y install /tmp/jre-8-linux-x64.rpm && \
    alternatives --install /usr/bin/java jar /usr/java/latest/bin/java 200000 && \
    rm -rf /tmp/jre-8-linux-x64.rpm

#Copies required build data
COPY $AEM_QUICKSTART_FILE $AEM_WORKING_DIR/$AEM_QUICKSTART_FILE
COPY license.properties $AEM_WORKING_DIR/license.properties

# Extracts AEM
WORKDIR $AEM_WORKING_DIR
RUN java -Xmx2024M -jar $AEM_QUICKSTART_FILE -unpack -r nosamplecontent -v

RUN mkdir -p $AEM_WORKING_DIR/crx-quickstart/install
COPY packages/ $AEM_WORKING_DIR/crx-quickstart/install/
COPY start $AEM_WORKING_DIR/crx-quickstart/bin/start

ENV JAVA_HOME /usr/java/latest

WORKDIR $AEM_WORKING_DIR
