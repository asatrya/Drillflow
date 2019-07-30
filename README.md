<img src="https://s3.us-east-2.amazonaws.com/hm-witsml-server/drillFlowLogo.png" alt="DrillFlow"/>

[![Codacy Badge](https://api.codacy.com/project/badge/Grade/90eb9da04f30402fb68f54c5225102e1)](https://www.codacy.com/app/cherrera2001/Drillflow?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=hashmapinc/Drillflow&amp;utm_campaign=Badge_Grade)
[![Build Status](https://travis-ci.org/hashmapinc/Drillflow.svg?branch=master)](https://travis-ci.org/hashmapinc/Drillflow)
[![Documentation Status](https://readthedocs.org/projects/drillflow/badge/?version=latest)](https://drillflow.readthedocs.io/en/latest/?badge=latest)
[![Docker Build Status](https://img.shields.io/docker/build/hashmapinc/drillflow.svg?logo=docker)](https://hub.docker.com/r/hashmapinc/drillflow/)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fhashmapinc%2FDrillflow.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Fhashmapinc%2FDrillflow?ref=badge_shield)

A dockerized WITSML API Server that is agnostic of the backend.

## Table of Contents

-   [Features](#features)
-   [Requirements](#requirements)
-   [Getting Started](#getting-started)
-   [Getting Help](#getting-help)
-   [Documentation](#documentation)
-   [License](#license)
-   [Export Control](#export-control)

## Features

Drillflow is an API facade that allows Oil and Gas software systems that currently expose drilling data to leverage WITSML to exchange data between other software systems and vendors. It is packaged as a docker image or a Spring Boot application to allow for minimum hassle at deploy time. It is also intended to be extensible and horizontally scalable to handle everything from a one time bulk load to a streaming application.

### WHY?!?!?

WITSML servers have been implemented in many forms and fashions over the relatively long lifespan of WITSML. The point of Drillflow is to ease the burden for software developers and system integrators to make use of WITSML data. Our goal is that if we can get this hurdle out of the way quicker, time to value is reduced.

Our goal was to implement a WITSML server on a modern stack: Java 11, Spring Boot with CXF, deployed with docker.

## Requirements

-   JDK 11 
-   Apache Maven 3.3 or higher
-   Git Client
-   Docker (To build the docker image)

## Getting Started

### Clone project & set working directory

```bash
git clone https://github.com/asatrya/Drillflow
cd Drillflow
```

### Check Java version

Make sure your Java version is Java 11

```bash
java -version
# output: openjdk version "11.0.3" 2019-04-16
```

If it's not, then change the version first

```bash
sudo update-alternatives --config java
```

### Building

Execute:

```bash
mvn clean install
```

### Running

Set environment variable:

```bash
export VALVE_API_KEY=secret

# url of JWT authentication
export TOKEN_PATH=http://localhost:8082/login
```

Execute:

```bash
java -jar df-server/target/df-server-0.0.1-SNAPSHOT.jar
```

### Testing

By default the service will be available at:

`http://localhost:7070/Service/WMLS`

#### Soap UI
Execute the following SOAP query to get the version:
```xml
<soapenv:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://www.witsml.org/message/120">
   <soapenv:Header/>
   <soapenv:Body>
      <ns:WMLS_GetVersion soapenv:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/>
   </soapenv:Body>
</soapenv:Envelope>
```
 
By default the server should return the following response:
 
 ```xml
<soap:Envelope xmlns:soap="http://schemas.xmlsoap.org/soap/envelope/">
    <soap:Body>
        <ns1:WMLS_GetVersionResponse xmlns:ns1="http://www.witsml.org/wsdl/120">
            <return xmlns:ns2="http://www.witsml.org/wsdl/120">1.3.1.1,1.4.1.1</return>
        </ns1:WMLS_GetVersionResponse>
    </soap:Body>
</soap:Envelope>
  ```

#### Postman

For Postman, use a POST query to the same URL as stated above with encoding type as `text/xml` and add a header with key `SOAPAction`and the value 
`http://www.witsml.org/action/120/Store.WMLS_GetVersion`. Then you can use the same query as above.

Your request from Postman should similar to this

```bash
curl -X POST \
  http://localhost:7070/Service/WMLS \
  -H 'Authorization: Basic YWRtaW46cGFzc3dvcmQ=' \
  -H 'Content-Type: text/plain' \
  -H 'SOAPAction: http://www.witsml.org/action/120/Store.WMLS_GetVersion' \
  -H 'cache-control: no-cache' \
  -d '<soapenv:Envelope xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:soapenv="http://schemas.xmlsoap.org/soap/envelope/" xmlns:ns="http://www.witsml.org/message/120">
   <soapenv:Header/>
   <soapenv:Body>
      <ns:WMLS_GetVersion soapenv:encodingStyle="http://schemas.xmlsoap.org/soap/encoding/"/>
   </soapenv:Body>
</soapenv:Envelope>'
```

### CVE Checking

Drillflow includes the [OWASP Dependancy checker](https://github.com/jeremylong/DependencyCheck) Maven Plugin

To check if any dependencies are subject to any current CVE's run

```bash
mvn clean install -Dowasp.skip=false
```

### Building the Docker image

Navigate to the docker directory

Execute `docker build . -t hashmapinc/drillflow:latest` to build the image

Once completed execute `docker run -p 7070:7070 hashmapinc/witsmlapi-server:latest` 

## Getting Help
You can also submit issues or questions via GitHub Issues [here](https://github.com/hashmapinc/WitsmlApi-Server/issues)

## Documentation

See [The Documentation Here](https://drillflow.readthedocs.io/en/latest/) for the latest updates.

## License

Except as otherwise noted this software is licensed under the
[Apache License, Version 2.0](http://www.apache.org/licenses/LICENSE-2.0.html)

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

  [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Fhashmapinc%2FDrillflow.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Fhashmapinc%2FDrillflow?ref=badge_large)

## Export Control

This distribution includes cryptographic software. The country in which you
currently reside may have restrictions on the import, possession, use, and/or
re-export to another country, of encryption software. BEFORE using any
encryption software, please check your country's laws, regulations and
policies concerning the import, possession, or use, and re-export of encryption
software, to see if this is permitted. See <http://www.wassenaar.org/> for more
information.

The U.S. Government Department of Commerce, Bureau of Industry and Security
(BIS), has classified this software as Export Commodity Control Number (ECCN)
5D002.C.1, which includes information security software using or performing
cryptographic functions with asymmetric algorithms. The form and manner of this
distribution makes it eligible for export under the
License Exception ENC Technology Software Unrestricted (TSU) exception (see the
BIS Export Administration Regulations, Section 740.13) for both object code and
source code.

The following provides more details on the included cryptographic software:

This project uses BouncyCastle and the built-in
java cryptography libraries for SSL, SSH via CXF. See
[http://bouncycastle.org/about.html](http://bouncycastle.org/about.html)
[http://www.oracle.com/us/products/export/export-regulations-345813.html](http://www.oracle.com/us/products/export/export-regulations-345813.html)
for more details on each of these libraries cryptography features.
