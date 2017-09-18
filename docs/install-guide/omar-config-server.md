# OMAR Config Server

## Dockerfile
```
FROM omar-base
COPY omar-config-server-1.2.0-SNAPSHOT.jar /home/omar
WORKDIR /home/omar
EXPOSE 8888
CMD java -Xms256m -Xmx1024m -Djava.security.egd=file:/dev/./urandom -jar /usr/share/omar/omar-config-server-1.2.0-SNAPSHOT.jar
```
Ref: [omar-base](../../../omar-base/docs/install-guide/omar-base/)

## JAR
[https://artifactory.ossim.io/artifactory/webapp/#/artifacts/browse/tree/General/omar-local/io/ossim/omar/apps/omar-config-server](https://artifactory.ossim.io/artifactory/webapp/#/artifacts/browse/tree/General/omar-local/io/ossim/omar/apps/omar-config-server)
