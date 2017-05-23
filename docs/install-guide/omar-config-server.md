# OMAR Config Server

## Dockerfile
```
FROM omar-ossim-base
RUN mkdir /usr/share/omar
COPY omar-config-server-1.0.0-SNAPSHOT.jar /usr/share/omar
RUN chown -R 1001:0 /usr/share/omar
RUN chown 1001:0 /usr/share/omar
RUN chmod -R g+rw /usr/share/omar
RUN find $HOME -type d -exec chmod g+x {} +
USER 1001
WORKDIR /usr/share/omar
VOLUME /dev/random /home /Users
EXPOSE 8888
CMD java -Xms256m -Xmx1024m -Dspring.profiles.active=production -Djava.security.egd=file:/dev/./urandom -Dserver.contextPath=/omar-config-server -jar /usr/share/omar/omar-config-server-1.0.0-SNAPSHOT.jar
```
Ref: [omar-ossim-base](../../../omar-ossim-base/docs/install-guide/omar-ossim-base/)

## JAR
`http://artifacts.radiantbluecloud.com/artifactory/webapp/#/artifacts/browse/tree/General/omar-local/io/ossim/omar/apps/omar-config-server`