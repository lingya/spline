## Get Spline
To get started, you need to get a minimal set of Spline's moving parts - 
a server, an admin tool and a client Web UI to see the captured lineage.

There are two ways how to do it:

#### Download prebuild Spline artifacts from the Maven repo
-   [```za.co.absa.spline:admin:0.4.0```](https://repo1.maven.org/maven2/za/co/absa/spline/admin/0.4.0/)
-   [```za.co.absa.spline:client-web:0.4.0```](https://repo1.maven.org/maven2/za/co/absa/spline/client-web/0.4.0/)

Download `*-exec.jar`'s from the above modules.

-or-

#### Build Spline from the source code
1.  Get and unzip the Spline source code:
    ```shell script
    wget https://github.com/AbsaOSS/spline/archive/release/0.4.0.zip
    unzip 0.4.0.zip
    ```
1.  Change the directory:
    ```shell script
    cd spline-release-0.4.0
    ```
1.  Run the Maven build:
    ```shell script
    mvn install -DskipTests
    ```

## Install ArangoDB
Spline server requires ArangoDB to run.
Please install _ArangoDB 3.5+_ according to the instructions [here](https://www.arangodb.com/docs/stable/getting-started-installation.html)

If you prefer a Docker image there is a [Docker repo](https://hub.docker.com/_/arangodb/) as well.
```shell script
docker pull arangodb:3.5.1
```

## Create Spline database
```shell script
java -jar admin/target/admin-0.4.0.jar db-init arangodb://localhost/spline
```

## Start Spline Server
The easiest way to spin up the Spline server is to use Docker:

```shell script
docker container run \
      -e spline.database.connectionUrl=arangodb://172.17.0.1/spline \
      -p 8080:8080 \
      absaoss/spline-rest-server
```

Or you can deploy it as a WAR-file into any Java compatible Web-Container, e.g. Tomcat.
You can find a WAR-file in the Maven repo here:
[```za.co.absa.spline:rest-gateway:0.4.0```](https://repo1.maven.org/maven2/za/co/absa/spline/rest-gateway/0.4.0/)

The server exposes the following REST API:
-   Producer API (`/producer/*`) 
-   Consumer API (`/consumer/*`)

... and other useful URLs:
-   Running server version information: [/about/build](http://localhost:8080/about/build)
-   Producer API Swagger documentation: [/docs/producer.html](http://localhost:8080/docs/producer.html) 
-   Consumer API Swagger documentation: [/docs/consumer.html](http://localhost:8080/docs/consumer.html) 

## Run Spline examples 
To run Spline example, download the Spline source code from GitHub and switch to the `examples` directory.     
```shell script
cd $SPLINE_PROJECT/examples
```

To run all available examples:
```shell script
mvn test -P examples
```

To run a selected example job (e.g. `Example1Job`):
```shell script
mvn test -P examples -D exampleClass=za.co.absa.spline.example.batch.Example1Job
``` 

To change the Spline Producer URL (default is http://localhost:8080/producer):
```shell script
mvn test -P examples -D spline.producer.url=http://localhost:8888/producer
```

## Run Spline UI
```shell script
java -jar client-web/target/spline-ui-0.4.0.exec.jar -httpPort 9090 -D spline.server.rest_endpoint=http://localhost:8080/consumer
```
and check the result in the browser
http://localhost:9090
 
---

    Copyright 2017 ABSA Group Limited
    
    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at
    
        http://www.apache.org/licenses/LICENSE-2.0
    
    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.