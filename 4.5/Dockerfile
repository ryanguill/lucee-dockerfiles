FROM tomcat:8-jre8

MAINTAINER Geoff Bowers <modius@daemon.com.au>

ENV LUCEE_JARS_URL https://d8yjolse1mixx.cloudfront.net/lucee-4.5.2.018-jars.zip

# Download jars archive, extract it, and delete it in one step to avoid committing the installer in a FS layer
RUN wget -nv $LUCEE_JARS_URL -O /root/lucee-jars.zip && \
	unzip /root/lucee-jars.zip -d /usr/local/tomcat/lucee && \
	rm -rf /root/lucee-jars.zip

# Delete the default Tomcat webapps so they aren't deployed at startup
RUN rm -rf /usr/local/tomcat/webapps/*

# Set Tomcat config to load Lucee
COPY catalina.properties server.xml web.xml /usr/local/tomcat/conf/

# Custom setenv.sh to load Lucee
COPY setenv.sh /usr/local/tomcat/bin/
RUN chmod a+x /usr/local/tomcat/bin/setenv.sh

# Create Lucee config dirs
RUN mkdir -p /opt/lucee/server/lucee-server/context /opt/lucee/web
COPY lucee-server.xml /opt/lucee/server/lucee-server/context/lucee-server.xml
COPY lucee-web.xml.cfm /opt/lucee/web/lucee-web.xml.cfm

# Create webroot with a default index.cfm
RUN mkdir -p /var/www
COPY index.cfm /var/www/

ONBUILD RUN rm -rf /var/www/*