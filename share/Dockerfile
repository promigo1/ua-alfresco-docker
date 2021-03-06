FROM keensoft/alfresco-docker-template:201605-GA
MAINTAINER keensoft.es

RUN set -x \
	&& yum update -y \
	&& yum clean all

WORKDIR $CATALINA_HOME
ENV ALF_HOME $CATALINA_HOME

# basic configuration
RUN set -x \
        && mkdir -p conf/Catalina/localhost \
        && mv $DIST/web-server/shared . \
        && mv $DIST/web-server/webapps/share.war webapps/share.war \
        && cp -r $DIST/bin . \
        && mv $DIST/licenses . \
        && mv $DIST/README.txt . \
        && rm -rf webapps/docs \
        && rm -rf webapps/examples \
        && mkdir shared/lib amps_share \
        && rm -rf $DIST 


COPY assets/tomcat/catalina.properties conf/catalina.properties
COPY assets/tomcat/setenv.sh bin/setenv.sh
COPY assets/share/share-config-custom.xml shared/classes/alfresco/web-extension/share-config-custom.xml
COPY assets/cas/java-cas-client.properties /etc/java-cas-client.properties
#COPY assets/cas/cacerts /usr/java/default/jre/lib/security/cacerts

# AMPS installation
COPY assets/share/apply_share_amps.sh bin/apply_amps.sh
COPY assets/amps_share amps_share
RUN bash ./bin/apply_amps.sh -nobackup -force

ENV PATH $ALF_HOME/bin:$PATH

RUN useradd -ms /bin/bash alfresco
RUN set -x && chown -RL alfresco:alfresco $ALF_HOME
USER alfresco

EXPOSE 8080
CMD ["catalina.sh", "run"]
