FROM onesaitplatform/baseimage:latest

# Metadata
LABEL platform.image.name="monitoringui"

ADD *-exec.jar app.jar

# Timezone 
RUN apk add --no-cache tzdata

# logs folder
RUN mkdir -p /var/log/platform-logs && \
    mkdir -p /usr/local/bin && \
	mkdir ./target
	
# Copy certs
COPY ocpserver.crt /usr/local/
COPY ocpserver.key /usr/local/

# create onesait user/group
RUN addgroup -S onesait -g 433 && adduser -u 431 -S -g onesait -h /usr/local -s /bin/sh onesait 

RUN chown -R onesait:onesait /usr/local && \
    chown -R onesait:onesait /var/log/platform-logs && \
    chown -R onesait:onesait ./target && \    
    chown onesait:onesait app.jar && \
    chmod -R 777 ./target && \
    chmod -R 777 /var/log && \
    chmod -R 777 /usr/local && \
    chmod -R 777 /etc/ssl/certs/java

VOLUME ["/tmp", "/var/log/platform-logs"]

EXPOSE 18100

ENV SERVER_NAME=localhost \
    CONFIGDBURL="jdbc:mysql://configdb:3306/onesaitplatform_config?useUnicode=true&characterEncoding=UTF-8&allowMultiQueries=true&useSSL=false&autoReconnect=true" \
    CONFIGDBUSER=root \
    CONFIGDBPASS=changeIt! \
    JDBCPROTOCOL="jdbc:mysql:" \
    DBADDPROPS="" \
    CLIENTID="onesaitplatform" \
    CLIENTSECRET="onesaitplatform" \
    PATH="/usr/local/bin:${PATH}" \
    ENVIRONMENTNAME="" \
    CONFIGDB_ACL_ENABLED=false \
    CONFIGDB_ACL_LIST=administrator,analytics,dataviewer,demo_developer,demo_user,developer,operations,partner,sysadmin,user
    


COPY docker-entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/docker-entrypoint.sh

USER onesait

ENTRYPOINT ["docker-entrypoint.sh"]