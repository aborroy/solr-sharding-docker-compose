# Alfresco SOLR Sharding Docker Compose Templates

Docker Compose deployment templates for Alfresco SOLR Sharding methods.

Documentation for Sharding Methods available in https://docs.alfresco.com/sie/concepts/solr-shard-approaches.html

* [acl_id](acl_id)
* [date](date)
* [db_id](db_id)
* [db_id_range](db_id_range)
* [explicit_id](explicit_id)
* [explicit_id_fallback_lris](explicit_id_fallback_lris)
* [lris](lris)
* [mod_acl_id](mod_acl_id)
* [property](property)

* [ssl_db_id](ssl_db_id): Sample on using DB_ID Sharding method configured with mTLS protocol

Every deployment includes 2 SOLR Servers (`solr6` and `solr62`) as sample implementation, but it can be used more if required.

Search Services Docker Image has been customized in order to apply the right properties for every Sharding Method.

Some sharding methods (`explicit_id`, `explicit_id_fallback_lris` and `property`) require using a custom Content Model. In order to deploy this model, Alfresco Docker Image has been customized.

## Base Template

SOLR Docker Containers include the ID of the Shard, the number of shards and other required information as Docker Image arguments in `docker-compose.yml` file.

```
solr6:
    build:
      context: ./search
      args:
        ALFRESCO_COMMS: none
        NUM_SHARDS: "2"
        SHARD_ID: "0"
```

Customization in `search/Dockerfile` modifies SOLR Core configuration in the `rerank` template in order to set the right values for the selected Sharding Method.

```
# Sharding ACL_ID
ARG NUM_SHARDS
ENV NUM_SHARDS $NUM_SHARDS
ARG SHARD_ID
ENV SHARD_ID $SHARD_ID

RUN sed -i '/^bash.*/i echo "\nshard.instance=${SHARD_ID}" >> ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh && \
    sed -i '/^bash.*/i echo "\nshard.count=${NUM_SHARDS}" >> ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh && \
    sed -i '/^bash.*/i sed -i "'"s/shard.method=DB_ID/shard.method=ACL_ID/g"'" ${DIST_DIR}/solrhome/templates/rerank/conf/solrcore.properties\n' \
    ${DIST_DIR}/solr/bin/search_config_setup.sh;
```

# How to use this composition

## Start Docker

Start docker and check the ports are correctly bound.

```bash
$ docker-compose up --build --force-recreate
$ docker ps --format '{{.Names}}\t{{.Image}}\t{{.Ports}}'

db_id_alfresco_1    db_id_alfresco                    8000/tcp
db_id_postgres_1    postgres:11.7                     0.0.0.0:5432->5432/tcp
db_id_activemq_1    alfresco/alfresco-activemq:5.15.8 0.0.0.0:5672->5672/tcp, ...
db_id_solr6_1       db_id_solr6                       0.0.0.0:8983->8983/tcp
db_id_solr62_1      db_id_solr62                      0.0.0.0:8984->8983/tcp
db_id_transform-core-aio_1	alfresco/alfresco-transform-core-aio:2.3.4	0.0.0.0:8090->8090/tcp
```

## Access

Use the following username/password combination to login in Alfresco services.

 - User: admin
 - Password: admin

Alfresco and related web applications can be accessed from the below URIs when the servers have started.

```
http://localhost:8080/alfresco      - Alfresco Repository (REST)
http://localhost:8983/solr          - Alfresco Search Services (Shard 1)
http://localhost:8984/solr          - Alfresco Search Services (Shard 2)
```
