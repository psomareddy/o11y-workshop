# Lab 2: Set up Pet Clinic Sample App (Demonstrating Zero Configuration Auto Instrumentation for Java)

First thing we need to setup APM is… well, an application. For this exercise, we will use the Spring PetClinic application. This is a very popular sample java application built with Spring framework (Springboot). We will also demonstrate the zero configuration feature of Open Telemetry Collector by showing that instrumentation is injected automatically into any Java application without any additional configuration.

## Set up Pet Clinic Sample App


Login into your EC2 machine. You should be in the "/home/ubuntu" folder. Verify that by running this command.
```cmd
pwd
```

Change directory to the petclinic application folder (workshop/petclinic), then we will compile, build, package and test the application.

```cmd
cd workshop/petclinic
```

Start a MySQL database (docker based) for PetClinic to use:

```cmd
./startdb.sh
```

Next, run the maven command to compile/build/package PetClinic (./mvnw package -Dmaven.test.skip=true):

```cmd
./build.sh
```

Once the compilation is complete, you can run the application with the following command:

The run script declares the OTEL_SERVICE_NAME and OTEL_RESOURCE_ATTRIBUTES and invokes the java start command.
Resource attributes specified here are added to every reported span from this application. So it is highly useful to add the version tag. 
A comma separated list of resource attributes can also be defined e.g. deployment.environment=${HOSTNAME},version=1.1.0
At a minimum, you should specify the **deployment.environment** resource attribute but others are optional.

```cmd
./run.sh
```

If you check the logs of the Splunk OpenTelemetry collector you will see that the collector automatically detected the application running and auto-instrumented it. You can view the logs using the following command:

```cmd
sudo tail -f /var/log/syslog
```

You can validate if the application is running by visiting http://<VM_IP_ADDRESS>:8080.

Once your validation is complete you can stop the application by pressing Ctrl-C.

## Generate Traffic

Next we will start a Docker container running Locust that will generate some simple traffic to the PetClinic application. Locust is a simple load testing tool that can be used to generate traffic to a web application.

```cmd
./locust.sh
```

## Start the app again
```cmd
./start.sh
```

## Next Step

[Go back to Main Page and Proceed to Lab 3](README.md)
