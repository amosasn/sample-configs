# Jetty 9 install

This package was created with these steps

## Basic map functionality

##### 1) Download Jetty as zip from https://www.eclipse.org/jetty/download.html (tested with 9.4.12)

Unzip to a location on your computer. The location will be referenced as {JETTY_HOME}.

##### 2) Cleanup the demo-content:

- Move ./{JETTY_HOME}/demo-base/ to ./oskari-server/ (oskari-server will be called {JETTY_BASE} from now on)
- Remove everything from etc, lib/ext, resources and webapps
- Remove start.d/demo.ini and start.d/https.ini

##### 3) Adding the PostgreSQL driver

- Download the driver .jar-file from https://jdbc.postgresql.org/download.html (tested with JDBC 4.2 Postgresql Driver, Version 42.2.5)
- Place the driver in {JETTY_BASE}/lib/ext/postgresql-42.2.5.jar

##### 4) Add configuration to serve Oskari frontend files

- add oskari-front.xml to {JETTY_BASE}/webapps/
- run 'git clone https://github.com/oskariorg/oskari-frontend' in {JETTY_BASE}
- after clone you have for example a file in {JETTY_BASE}/oskari-frontend/ReleaseNotes.md
- optionally modify 'resourceBase' in oskari-front.xml to point to a location where Oskari frontend files are located

##### 5) Configuring oskari-map as root webapp

- add oskari-map.xml to {JETTY_BASE}/webapps/
- add oskari-ext.properties to {JETTY_BASE}/resources/
- configure the database connection parameters (user/password) in oskari-ext.properties
- edit oskari.domain property in oskari-ext.properties if you are not running in port 8080
- run 'git clone https://github.com/oskariorg/oskari-server' on your computer to get the latest code for oskari-server
- build the webapp by calling "mvn clean install" in the root of the oskari-server repository to compile webapp-map/target/oskari-map.war file and copy it to {JETTY_BASE}/webapps/oskari-map.war
- to add WFS-support copy webapp-transport/target/transport.war file to {JETTY_BASE}/webapps/transport.war

##### 6) Start the Jetty by running the command in {JETTY_HOME}

	java -jar $JETTY_HOME/start.jar jetty.base=$JETTY_BASE jetty.http.port=8080

This creates the basic database structure (if it doesn't exist) with initial content based on a json file in content-resources modules resources. Overridable by having the setup file inside webapp-map.

#### Note! 

Running the transport webapp also requires Redis to be installed in addition to PostgreSQL/PostGIS.

##### 1) Download Redis from http://redis.io/download

##### 2) Start redis-server with default config (localhost:6379)

Configuration instructions for non-default settings TBD.

##### 3) Configure webapp-map location for transport (optional)

- add transport-ext.properties to {JETTY_BASE}/resources/
- edit oskari.domain property in transport-ext.properties if you are not running in port 8080

	oskari.domain=http://localhost:8080

##### 4) Restart Jetty.