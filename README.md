# Boost Maven Plugin Tutorial

This is a basic walkthrough of how to use the Boost Maven Plugin to package an existing J2EE or Microprofile application into a Liberty executable jar.

### Create an Application project

To get started, you will first need a basic J2EE or Microprofile application. You can clone the following project as an instructional guide:

`git clone git@github.com:OpenLiberty/boosted-microprofile-rest-client.git`

The application is a simple MicroProfile application that uses MP RestClient and MP Config to retrieve and display system properties for a target server. The application also uses JPA services to track an invocation counter and timestamp value.

The application project's pom.xml has been configured to use the boost plugin and the boost technology booster dependencies.

### Package your application

Run the Maven package command as you normally would when building a Java EE or MicroProfile Application:

* `mvn clean package -Dboost.http.port=9000`

From the pom.xml, you can swap out the Apache Derby dependency for a MySQL or DB2 dependency and pass in the JDBC url, username and password. For example:

* `mvn clean package -Dboost.http.port=9000 -Dboost.db.url=jdbc:mysql://localhost:3306/testdb -Dboost.db.user=mysql -Dboost.db.password=mysql`

From the pom.xml, you can also swap out the Open Liberty runtime for a TomEE runtime by changing the artifact id of the "boost.runtimes" dependency:

openliberty -> tomee 

### Run the Application

You can run your application using the provided boost goals: 

`mvn boost:start`, `mvn boost:stop`, `mvn boost:run`, and `mvn boost:debug`. 

After you start the application, you can access the following microservices:

* The http://localhost:9080/system/properties  microservice simulates the remote `system` service that retrieves the system property information for a specific host. In this case, `localhost` is a specific host name.

* The http://localhost:9080/inventory/systems/localhost microservice is the `inventory` service that invokes the http://localhost:9080/system/properties microservice to retrieves the system property information.

* The http://localhost:9080/inventory/systems/{your_hostname} microservice is the `inventory` service that invokes the http://{your_hostname}:9080/system/properties microservice. In Windows, Mac OS, and Linux, get your fully qualified domain name (FQDN) by entering `hostname` from your terminal. Visit the URL by replacing `{your_hostname}` with your FQDN.
You will see the same system property information, but the process of getting the information is different.
