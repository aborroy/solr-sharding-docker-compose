FROM quay.io/alfresco/search-services:2.0.0

# Sharding DB_ID
ARG NUM_SHARDS
ENV NUM_SHARDS $NUM_SHARDS
ARG SHARD_ID
ENV SHARD_ID $SHARD_ID

RUN sed -i '/^bash.*/i echo "\nshard.instance=${SHARD_ID}" >> ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh && \
    sed -i '/^bash.*/i echo "\nshard.count=${NUM_SHARDS}" >> ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh && \
    sed -i '/^bash.*/i sed -i "'"s/shard.method=DB_ID/shard.method=DB_ID/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh;

RUN sed -i '/^bash.*/i \
    sed -i "'"s/alfresco.encryption.ssl.keystore.location=.*/alfresco.encryption.ssl.keystore.location=\\\/opt\\\/alfresco-search-services\\\/keystores\\\/ssl-repo-client.keystore/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties && \
    sed -i "'"s/alfresco.encryption.ssl.keystore.passwordFileLocation=.*/alfresco.encryption.ssl.keystore.passwordFileLocation=/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties && \
    sed -i "'"s/alfresco.encryption.ssl.keystore.type=.*/alfresco.encryption.ssl.keystore.type=JCEKS/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties && \
    sed -i "'"s/alfresco.encryption.ssl.truststore.location=.*/alfresco.encryption.ssl.truststore.location=\\\/opt\\\/alfresco-search-services\\\/keystores\\\/ssl-repo-client.truststore/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties && \
    sed -i "'"s/alfresco.encryption.ssl.truststore.passwordFileLocation=.*/alfresco.encryption.ssl.truststore.passwordFileLocation=/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties && \
    sed -i "'"s/alfresco.encryption.ssl.truststore.type=.*/alfresco.encryption.ssl.truststore.type=JCEKS/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh;

VOLUME ["${DIST_DIR}/keystores"]
