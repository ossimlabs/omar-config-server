# omar-config-server

The OMAR Spring Configuration Server is used to hand configuration files out to the various OMAR Spring micro services.

The config-server can be used in two primary profile configurations:
1. native

   Under the **native** profile the server will look for application configurations placed in the ${HOME}/configs directory.

   Note, if load balancing the service this storage location must be shared by all instances of the service. For more information, consult the [Spring Cloud Config Page](https://cloud.spring.io/spring-cloud-config/spring-cloud-config.html)
2. remote

   With the **remote** profile the server will search for configurations using a Git backend. By default, the Git backend uses the [radiantbluetechnologies/config-repo](https://github.com/radiantbluetechnologies/config-repo). However, this can be overridden with the spring.cloud.config.server.git.uri parameter.

   In addition, the environment variables GIT_USER_NAME and GIT_PASSWORD have to be set in order for the server to pick up the radiantbluetechnologies/config-repo. Again, these properties can be overridden with spring.cloud.config.server.git.username and spring.cloud.config.server.git.password respectively. See the [Spring Cloud Config Page](https://cloud.spring.io/spring-cloud-config/spring-cloud-config.html) for more information.
