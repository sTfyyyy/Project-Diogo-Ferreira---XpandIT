# Use CentOS 7 as the base
FROM centos:7

# Install all the necessary packages
RUN yum -y install java-1.8.0-openjdk wget openssl

# Create directory for SSL
RUN mkdir -p /etc/ssl/private

# Generate a self-signed SSL certificate
RUN openssl req -new -newkey rsa:2048 -days 365 -nodes -x509 \
  -subj "/C=PT/ST=Portugal/L=Lisboa/O=XPandIT/CN=projetodiogoferreira.com" \
  -keyout /etc/ssl/private/server.key -out /etc/ssl/certs/server.crt

# Download and install Tomcat 8.5
RUN wget https://archive.apache.org/dist/tomcat/tomcat-8/v8.5.73/bin/apache-tomcat-8.5.73.tar.gz && \
  tar xzf apache-tomcat-8.5.73.tar.gz -C /opt && \
  mv /opt/apache-tomcat-8.5.73 /opt/tomcat && \
  rm -f apache-tomcat-8.5.73.tar.gz

# Create a JKS keystore with password
RUN openssl pkcs12 -export -in /etc/ssl/certs/server.crt \
  -inkey /etc/ssl/private/server.key -out /etc/ssl/server.p12 \
  -name tomcat -passout pass:projetodiogoferreira

RUN keytool -importkeystore -deststorepass projetodiogoferreira \
  -destkeypass projetodiogoferreira -destkeystore /etc/ssl/server.jks \
  -srckeystore /etc/ssl/server.p12 -srcstoretype PKCS12 \
  -srcstorepass projetodiogoferreira -alias tomcat -noprompt

# Remove temporary keystore
RUN rm /etc/ssl/server.p12

# Update Tomcat server.xml for SSL using 4041 as 443
RUN sed -i '/<\/Service>/ i \
<Connector port="4041" protocol="HTTP/1.1" SSLEnabled="true" \
           maxThreads="150" scheme="https" secure="true" \
           clientAuth="false" sslProtocol="TLS" \
           keystoreFile="/etc/ssl/server.jks" \
           keystorePass="projetodiogoferreira" \
           keyAlias="tomcat"/>' /opt/tomcat/conf/server.xml

# Copy the application WAR file into the Tomcat webapp file
COPY sample.war /opt/tomcat/webapps/

# Expose port
EXPOSE 4041

#Simple script to start Tomcat
RUN echo '#!/bin/bash' > /usr/local/bin/start_services.sh && \
    echo '/opt/tomcat/bin/catalina.sh run' >> /usr/local/bin/start_services.sh && \
    chmod +x /usr/local/bin/start_services.sh

# Start Tomcat
CMD ["/usr/local/bin/start_services.sh"]
