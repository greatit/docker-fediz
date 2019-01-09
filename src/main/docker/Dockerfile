FROM tomcat:9.0.14-jre8-alpine

ARG FEDIZ_VERSION=1.4.4
ARG TOMCAT_HOME=/usr/local/tomcat
ARG WAR_LOCATION=/tmp/fediz-idp-sts-${FEDIZ_VERSION}.war
ARG FEDIZ_HOME=${TOMCAT_HOME}/webapps/fediz-idp-sts

LABEL version=${FEDIZ_VERSION}
EXPOSE 8443/tcp
WORKDIR /tmp

ADD http://central.maven.org/maven2/org/apache/cxf/fediz/fediz-idp-sts/${FEDIZ_VERSION}/fediz-idp-sts-${FEDIZ_VERSION}.war ${WAR_LOCATION} 
COPY server.xml ${TOMCAT_HOME}/conf
COPY keystore.jks ${TOMCAT_HOME}/conf

# explode Web archive for customization purposes
RUN mkdir ${TOMCAT_HOME}/webapps/fediz-idp-sts && \
 unzip ${WAR_LOCATION} -d ${FEDIZ_HOME} && \
 rm ${WAR_LOCATION}
 
# remove demo config
RUN rm -f ${FEDIZ_HOME}/WEB-INF/classes/*.cert \
 ${FEDIZ_HOME}/WEB-INF/classes/*.jks \
 ${FEDIZ_HOME}/WEB-INF/classes/stsKeystore*.properties \
 ${FEDIZ_HOME}/WEB-INF/classes/stsTruststore.properties
COPY WEB-INF/cxf-transport.xml ${FEDIZ_HOME}/WEB-INF/cxf-transport.xml
COPY WEB-INF/kerberos.jaas ${FEDIZ_HOME}/WEB-INF/kerberos.jaas
COPY WEB-INF/classes/* ${FEDIZ_HOME}/WEB-INF/classes/
COPY WEB-INF/data/* ${FEDIZ_HOME}/WEB-INF/data/
COPY WEB-INF/endpoints/* ${FEDIZ_HOME}/WEB-INF/endpoints/
COPY WEB-INF/wsdl/* ${FEDIZ_HOME}/WEB-INF/wsdl/
