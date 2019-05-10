# EmptyDateTest

## Setup
Load */SoapUI/EmptyDateService-soapui-project.xml* in SoapUI and run the **EmptyDateServiceSOAP MockService**.  The mock service should be exposed on port 8088 (e.g. [http://<host_name>:8088/EmptyDateServiceSOAP])

## Liberty Container
Update the JVM parameter **empty.date.soap.url** with the mock service URL (see Setup) in the *jvm.options* file located @ */WLP/emptyDateServer/jvm.options*.

*TIP: Use fully qualified DNS name or IP address of the host machine in order for WLP running inside the container to access the mock service running on the host machine (i.e. outside of the container).*

Use the following scripts as appropriate:
1. build.sh - Builds the **empty-date** docker image, will remove any running container named **empty-date-test** first.
2. run.sh - Runs the **empty-date** docker image with the container name **empty-date-test**.
3. logs.sh - Tails the WLP log of the **empty-date-test** container.

Open the URL in a browser to test: http://localhost:9081/EmptyDateTest/TestEmptyDateService

The mock service provides 2 different response.  One should return a valid date of 2019-05-04, the other should trigger the IllegalArgumentException.

Behavior is consistent with 18.0.0.4 and 19.0.0.4

## tWAS
WAR file provided for tWAS and tested against tWAS 8.5.5.13 with IBM SDK 8 (Docker Hub image).  The <emptyDate/> in the SOAP response results in a **null** object instead of the IllegalArgumentException.

Simply deploy the */tWAS/EmptyDateTest.war* into a tWAS 8.5.5.x server and set the custom JVM property **empty.date.soap.url** to the SoapUI mock service URL.  

Open the URL in a browser to test: http://<twas_hostname>:<twas_http_port>/EmptyDateTest/TestEmptyDateService

## Notes:
The WAR file contains source code for the test servlet.
