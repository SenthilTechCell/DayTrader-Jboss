## Setup Instructions for Windows

**Requirements**

 - Windows 10/11
 - Git
 - JDK 8
 - Maven
 - MySQL 8+
 - MySQL JDBC Connector
 - JBoss EAP 7.40
 
 **Steps for local setup**
 
 - Install Git for Windows
 - Clone this repository in your local machine
 - Install JDK 8 & Maven
 - Setup the system variables for Java & Maven
 - Install MySQL 8
 - Create database **tradedb** in MySQL 
 - Install JBoss EAP 7.40 using the executable jar and setup the JDBC Driver and Datasource as well during the installation process.
Following blocks should appear in `standalone/configuration/standalone-full.xml` file after setup. *Note: You will be required to specify location of a local copy of MySQL connector JAR file.*
- For MySQL Driver:
```
<driver name="mysql" module="com.mysql">
     <driver-class>com.mysql.jdbc.Driver</driver-class>
     <xa-datasource-class>com.mysql.jdbc.jdbc2.optional.MysqlXADataSource</xa-datasource-class>
</driver>
```
- For Datasource:
```
<datasource jndi-name="java:/jdbc/TradeDataSource" pool-name="trader" enabled="true">
     <connection-url>jdbc:mysql://localhost:3306/tradedb</connection-url>
     <driver>mysql</driver>
     <security>
         <user-name>daytrader</user-name>
         <password>daytrader</password>
     </security>
     <validation>
         <exception-sorter class-name="org.jboss.jca.adapters.jdbc.extensions.mysql.MySQLExceptionSorter"/>
     </validation>
</datasource>
```
- Goto cloned repository folder and run following command 
	 `mvn clean install` 
- The result of maven build is an EAR file located in the `javaee6/assemblies/daytrader-ear/target` directory which can be copied to JBoss EAP's `standalone/deployments` directory.
- Also, copy `setup/jboss-jms.xml` to JBoss EAP's `standalone/deployments` directory.
- Then, startup JBoss EAP using the  _Full_  profile, e.g.

```
bin/standalone.bat -c standalone-full.xml
```

- And access the app using  `http://<hostname>/daytrader`, for example  `http://localhost:8080/daytrader`

