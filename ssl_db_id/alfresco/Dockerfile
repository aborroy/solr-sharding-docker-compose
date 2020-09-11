FROM alfresco/alfresco-content-repository:6.3.0-A10

ARG TOMCAT_DIR=/usr/local/tomcat
ARG ALF_DATA_DIR=${TOMCAT_DIR}/alf_data

# Expose keystore folder
# Useless for 'none'/'http' communications with SOLR
VOLUME ["${ALF_DATA_DIR}/keystore"]

USER root

# Default value in "repository.properties" is "dir.keystore=classpath:alfresco/keystore"
RUN echo -e "\n\
    dir.keystore=${ALF_DATA_DIR}/keystore\n\
    alfresco.encryption.ssl.keystore.type=JCEKS\n\
    alfresco.encryption.ssl.truststore.type=JCEKS\n\
    " >> ${TOMCAT_DIR}/shared/classes/alfresco-global.properties;

# Enable SSL by adding the proper Connector to server.xml
RUN sed -i "s/\
[[:space:]]\+<\/Engine>/\n\
        <\/Engine>\n\
        <Connector port=\"8443\" protocol=\"org.apache.coyote.http11.Http11Protocol\"\n\
            connectionTimeout=\"20000\"\n\
            SSLEnabled=\"true\" maxThreads=\"150\" scheme=\"https\"\n\
            keystoreFile=\"\/usr\/local\/tomcat\/alf_data\/keystore\/ssl.keystore\"\n\
            keystorePass=\"kT9X6oe68t\" keystoreType=\"JCEKS\" secure=\"true\"\n\
            truststoreFile=\"\/usr\/local\/tomcat\/alf_data\/keystore\/ssl.truststore\"\n\
            truststorePass=\"kT9X6oe68t\" truststoreType=\"JCEKS\" clientAuth=\"want\" sslProtocol=\"TLS\">\n\
        <\/Connector>/g" ${TOMCAT_DIR}/conf/server.xml
