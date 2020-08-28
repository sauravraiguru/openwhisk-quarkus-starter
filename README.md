# Serverless Java Functions via Quarkus for Apache OpenWhisk

This project contains a simple sample how to use [Quarkus](https://quarkus.io/) to build functions for the serverless platform [Apache OpenWhisk](https://openwhisk.apache.org/).

**Download project**

```
$ git clone https://github.com/sauravraiguru/openwhisk-quarkus-starter.git
$ cd openwhisk-quarkus-starter
```

**Install prerequisites**

Before the actual Docker image is built, the native binary is created. Follow the [instructions](https://quarkus.io/guides/building-native-image-guide) on the Quarkus web site how to set up GraalVM, a Java JDK and Maven. 

**Build the image**

Replace 'sauravraiguru' with your Docker name.

```
$ mvn package -Pnative -Dnative-image.docker-build=true
$ docker build -t sauravraiguru/quarkus .
$ docker push sauravraiguru/quarkus1
```
Note: If you want to skip building the java-based Quarkus container image, you can pull the image from here 

```
$ docker pull sauravraiguru/quarkus
```
**Invoke the function locally**

```
$ docker run -i --rm -p 8080:8080 sauravraiguru/quarkus
$ curl --request POST \
  --url http://localhost:8080/run \
  --header 'Content-Type: application/json' \
  --data '{"value":{"name":"Saurav"}}'
```

**Develop the function locally [Optional]**

In order to change the implementation of the sample function, use your favorite Java IDE or text editor. When you run the following command, the application will be updated automatically every time you save a file:

```
$ mvn compile quarkus:dev
```

**Get an IBM Cloud account**

In order to run the function on IBM Cloud Functions,
- you need a free [IBM Cloud lite](https://cloud.ibm.com/registration) account and 
- the ‘[ibmcloud](https://console.bluemix.net/docs/cli/index.html)‘ CLI.
- Install IBM Functions CLI:

```
ibmcloud plugin install cloud-functions
```


**Setting up IBM Cloud & Functions**

```
$ ibmcloud login
```
Note: Enter the email & password to login

Target a CF region in your IBM Cloud account 
```
ibmcloud target --cf
```
Target the 'Default' resource group
```
ibmcloud target -g Default
```

**Create the OpenWhisk action**

Target the Function namespace:

https://cloud.ibm.com/functions/learn/cli

Create the OpenWhisk action
```
$ ibmcloud fn action create echo-quarkus --docker sauravraiguru/quarkus -m 128
```

**Invoke the OpenWhisk function**

```
$ ibmcloud fn action invoke --blocking echo-quarkus --param name Saurav
```
