FROM ubuntu:14.04

# Environment Variables
ENV FEDORA_VERSION 4.0.0
ENV TOMCAT_VERSION 7.0.59
ENV TOMCAT apache-tomcat-$TOMCAT_VERSION
ENV TOMCAT_HOME /usr/lib/$TOMCAT

RUN apt-get update && apt-get -y install \
        unzip \
        wget \
        default-jdk \
        git

WORKDIR /usr/lib

# Fetch Apache Tomcat
RUN wget -q -nc http://archive.apache.org/dist/tomcat/tomcat-7/v$TOMCAT_VERSION/bin/$TOMCAT.zip && \
	unzip -q /usr/lib/$TOMCAT.zip -d /usr/lib

# Fetch Fedora
RUN wget -q -nc http://repo1.maven.org/maven2/org/fcrepo/fcrepo-webapp/4.0.0/fcrepo-webapp-4.0.0.war

#Setting up Tomcat with Fedora
RUN cp /usr/lib/fcrepo-webapp-4.0.0.war /usr/lib/$TOMCAT/webapps/fedora.war

RUN mkdir /opt/fedora-data && \
	echo 'JAVA_OPTS="${JAVA_OPTS} -Dfcrepo.home=/opt/fedora-data"' >> /etc/default/tomcat7

ADD tomcat-users.xml /usr/lib/$TOMCAT/conf/tomcat-users.xml

RUN chmod +x /usr/lib/$TOMCAT/bin/*.sh

EXPOSE 8080

CMD /usr/lib/$TOMCAT/bin/catalina.sh run
