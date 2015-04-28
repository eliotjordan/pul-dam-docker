FROM ubuntu:14.04

# Environment Variables
ENV SOLR_VERSION 4.10.4
ENV SOLR solr-$SOLR_VERSION

RUN apt-get update && apt-get -y install \
        unzip \
        wget \
        default-jdk \
        git

WORKDIR /opt

# Fetch Apache Tomcat
RUN wget http://www.gtlib.gatech.edu/pub/apache/tomcat/tomcat-7/v7.0.61/bin/apache-tomcat-7.0.61.zip
RUN unzip apache-tomcat-7.0.61.zip
RUN mv apache-tomcat-7.0.61 /usr/share/tomcat7

# Fetch Solr
RUN wget http://www.gtlib.gatech.edu/pub/apache/lucene/solr/$SOLR_VERSION/$SOLR.tgz
RUN tar xzvf $SOLR.tgz
RUN mv solr-4.10.4 /usr/share/solr
RUN rm $SOLR.tgz

WORKDIR /usr/share
RUN git clone https://github.com/projecthydra/hydra-jetty.git
RUN cp -r  hydra-jetty/solr/ solr/hydra
RUN cp solr/example/webapps/solr.war solr/hydra/solr.war
RUN cp -r solr/example/lib/ext/* tomcat7/lib/
RUN cp -r solr/example/resources/log4j.properties tomcat7/lib/

ADD solr.xml /usr/share/tomcat7/conf/Catalina/localhost/solr.xml
ADD tomcat-users.xml /usr/share/tomcat7/conf/tomcat-users.xml
ADD apache_tomcat_setenv.sh /usr/share/tomcat7/bin/apache_tomcat_setenv.sh

RUN chmod 755 -R /usr/share/solr
RUN chmod +x /usr/share/tomcat7/bin/*.sh

EXPOSE 8080

CMD /usr/share/tomcat7/bin/catalina.sh run
