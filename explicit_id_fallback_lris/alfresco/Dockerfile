FROM alfresco/alfresco-content-repository:6.2.2

ARG TOMCAT_DIR=/usr/local/tomcat

# Copy custom content model to deployment folder
COPY model/* $TOMCAT_DIR/shared/classes/alfresco/extension/
