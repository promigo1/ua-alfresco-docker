FROM keensoft/alfresco-docker-template:201605-GA
MAINTAINER keensoft.es

RUN set -x \
	&& yum update -y \
	&& yum clean all

RUN set -x \
	&& yum install -y \
	    ImageMagick \
   	    ghostscript \
	&& yum clean all

WORKDIR $CATALINA_HOME
ENV ALF_HOME $CATALINA_HOME

# basic configuration
RUN set -x \
	&& mkdir -p conf/Catalina/localhost shared/classes/alfresco/extension \
#	&& mv $DIST/web-server/shared . \
	&& mv $DIST/web-server/webapps/alfresco.war webapps/alfresco.war \
	#&& mv $DIST/web-server/webapps/share.war webapps/share.war \
	&& wget https://artifacts.alfresco.com/nexus/service/local/repositories/releases/content/org/alfresco/share/5.1.g/share-5.1.g.war -O webapps/share.war \
	&& mv $DIST/web-server/lib/*.jar lib/ \
	&& mv $DIST/alf_data . \
	&& mv $DIST/amps . \
	&& cp -r $DIST/bin . \
	&& mv $DIST/licenses . \
	&& mv $DIST/README.txt . \
	&& rm -rf webapps/docs \
	&& rm -rf webapps/examples \
	&& rm -rf /tmp/alfresco \
	&& mkdir -p shared/lib amps_share \
	&& rm amps/alfresco-share-services.amp


COPY assets/tomcat/catalina.properties conf/catalina.properties
COPY assets/tomcat/setenv.sh bin/setenv.sh
COPY assets/alfresco/alfresco-global.properties shared/classes/alfresco-global.properties
COPY assets/cas/java-cas-client.properties /etc/java-cas-client.properties
COPY assets/share/share-config-custom.xml shared/classes/alfresco/web-extension/share-config-custom.xml

# AMPS installation
COPY assets/alfresco/apply_alfresco_amps.sh bin/apply_amps.sh
COPY assets/amps amps
RUN bash ./bin/apply_amps.sh -nobackup -force

COPY assets/share/apply_share_amps.sh bin/apply_share_amps.sh
COPY assets/amps_share amps_share
RUN bash ./bin/apply_share_amps.sh -nobackup -force

ENV PATH $ALF_HOME/bin:$PATH
ENV LANG en_US.utf8

RUN useradd -ms /bin/bash alfresco
RUN set -x && chown -RL alfresco:alfresco $ALF_HOME
USER alfresco

VOLUME $ALF_HOME/alf_data

EXPOSE 8080
CMD ["catalina.sh", "run"]
