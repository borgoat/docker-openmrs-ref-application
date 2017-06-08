# OpenMRS Reference Application

OpenMRS provides a base [Platform](https://github.com/giorgioazzinnaro/docker-openmrs-platform), and a reference implementation with default modules, which is the reference application.

## Configuration

This runs on top of [tomcat:8-jre8-alpine](https://hub.docker.com/_/tomcat/).

OpenMRS depends on [mysql:5.6](https://hub.docker.com/_/mysql/) as database.

```
docker run --name openmrs-mysql -e MYSQL_ROOT_PASSWORD=my-secret-pw -d mysql:5.6
```

Now run one `giorgioazzinnaro/openmrs-platform` instance linking it to `openmrs-mysql` (keep your MySQL password in mind for first configuration) and expose port 8080.

```
docker run --name openmrs-reference-application --link openmrs-mysql:mysql -it -p 8080:8080 giorgioazzinnaro/openmrs-reference-application:2.6.0
```

Point your browser to [localhost:8080/openmrs](http://localhost:8080/openmrs), and select `Advanced` installation.  
Change your database connection string to look like this:

```
jdbc:mysql://mysql:3306/@DBNAME@?autoReconnect=true&sessionVariables=default_storage_engine=InnoDB&useUnicode=true&characterEncoding=UTF-8
```
*note `localhost` was replaced with `mysql`*

Go on selecting the database to be created:

*Do you currently have an OpenMRS database installed that you would like to connect to?*: `No`  
*username*: root  
*password*: `$MYSQL_ROOT_PASSWORD` (*the password you selected for your MySQL container*)

On the next page, again enter `$MYSQL_ROOT_PASSWORD` so that the installer creates the new user.

Now change all other settings as desired.

**After configuration Tomcat will complain with some errors, this is because it should be restarted to load all the modules**

Keep MySQL running, stop openmrs-reference-application and start it again.  
Press `<Ctrl>-C` in the shell where it was running, then:

```
docker run --name openmrs-reference-application --link openmrs-mysql:mysql -it -p 8080:8080 giorgioazzinnaro/openmrs-reference-application:2.6.0
```


## docker-compose

For ease of use, a `docker-compose.yml` file is provided in the GitHub repo.
