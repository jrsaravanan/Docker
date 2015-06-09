FROM ubuntu:latest

MAINTAINER Saravanan Renganathan

# Install latest updates
RUN apt-get update
RUN apt-get upgrade -y

# Install mysql client and server
RUN apt-get -y install mysql-client mysql-server curl

# Enable remote access (default is localhost only, we change this
# otherwise our database would not be reachable from outside the container)
RUN sed -i -e"s/^bind-address\s*=\s*127.0.0.1/bind-address = 0.0.0.0/" /etc/mysql/my.cnf

# Install database
ADD ./database.sql /var/db/database.sql

# Set Standard settings
ENV user student
ENV password secret
ENV url file:/var/db/database.sql
ENV right READc24be954c5c5

# Install starting script
ADD ./start-database.sh /usr/local/bin/start-database.sh
#RUN chmod +x /usr/local/bin/start-database.sh


#EXPOSE 3306
#CMD ["/usr/local/bin/start-database.sh"]


# Required packages
RUN apt-get -qq update && apt-get -qq -y install software-properties-common curl

RUN /usr/bin/add-apt-repository ppa:webupd8team/java 2> /dev/null &&\
    apt-get update -qq

# Accept oracle-java7 license
RUN echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections

# Install java7 + mysql
RUN apt-get install -qq -y oracle-java7-installer 2> /dev/null


    
# Tomcat Version
ENV TOMCAT_VERSION_MAJOR 7
ENV TOMCAT_VERSION_FULL  7.0.57

# Download and install tomcat
RUN curl -kLO https://archive.apache.org/dist/tomcat/tomcat-${TOMCAT_VERSION_MAJOR}/v${TOMCAT_VERSION_FULL}/bin/apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz &&\
    tar -xzf apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz -C /opt &&\
    ln -s /opt/apache-tomcat-${TOMCAT_VERSION_FULL} /opt/tomcat &&\
    rm apache-tomcat-${TOMCAT_VERSION_FULL}.tar.gz &&\
    rm -rf /opt/tomcat/webapps/examples /opt/tomcat/webapps/docs


# Configure users for tomcat manager and host-manager
ADD conf/tomcat-users.xml /opt/tomcat/conf/
ADD war/person.war /opt/tomcat/webapps/person.war

# Set CATALINA_HOME
ENV CATALINA_HOME /opt/tomcat

# Launch Tomcat on startup
#CMD ${CATALINA_HOME}/bin/startup.sh && tail -f ${CATALINA_HOME}/logs/catalina.out


EXPOSE 8080

RUN chmod +x /usr/local/bin/start-database.sh

EXPOSE 3306


CMD ["/usr/local/bin/start-database.sh"]


