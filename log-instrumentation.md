# Lab 5: Configure application logs and forward them with Otel Collector

We will configure the Spring PetClinic application to write logs to a file in the filesystem and configure the Splunk OpenTelemetry Collector to read (tail) that log file and report the information to the Splunk Observability Platform.

## Application Log File
Our petclinic application uses the logback logging library to print logs to the /home/ubuntu/workshop/petclinic/petclinic.log file location.
Importantly the Java auto instrumentation instruments the logback logging library.

## Open Telemetry Configuration

The Splunk OpenTelemetry Collector uses the Filelog receiver to consume logs. We will need to edit the collectors configuration file:

```cmd
sudo vi /etc/otel/collector/agent_config.yaml
```

Under receivers: create the Filelog Receiver (make sure to indent correctly):

```
receivers:
    filelog:
        include: [/home/ubuntu/workshop/petclinic/petclinic.log]
```

The under the service: section, find the logs: pipeline, replace fluentforward with filelog and optionally remove otlp (again, make sure to indent correctly):

```
    logs:
        receivers: [filelog]
```

Save the file and exit the editor. 

Next, we need to edit the HEC Token and HEC URL for the collector to use. We will edit the /etc/otel/collector/splunk-otel-collector.conf file:

```cmd
sudo vi /etc/otel/collector/splunk-otel-collector.conf
```

Replace the values for SPLUNK_HEC_URL and SPLUNK_HEC_TOKEN, then restart the collector to apply the changes:


```cmd
sudo systemctl restart splunk-otel-collector
```

## Sample Code adding Logs 

Pet Clinic does not generate any application logs for user requests. So we add some code to generate log entries.

Open the .../***src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java*** file.

Add this import statement (import statements are usually found at the top of the java source file)
```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
```

Add a static logger field to the **OwnerController** class
```java
private static final Logger logger = LoggerFactory.getLogger(OwnerController.class);
```

Find the following one or more log entries in the **OwnerController.java** file.
```java
@GetMapping("/owners/{ownerId}")
public ModelAndView showOwner(@PathVariable("ownerId") int ownerId) {
    ModelAndView mav = new ModelAndView("owners/ownerDetails");
    Owner owner = this.owners.findById(ownerId);

    Span currentSpan = Span.current();
    currentSpan.setAttribute("custom.owner.id", owner.getId());
    currentSpan.setAttribute("custom.owner.name", String.join(" ", owner.getFirstName(), owner.getLastName()));
    currentSpan.setAttribute("custom.owner.city", owner.getCity());
    currentSpan.setAttribute("custom.owner.phone", owner.getTelephone());

    currentSpan.addEvent(
            String.format("custom event at petclinic: an owner %s's record was viewed", owner.getFirstName()));

    mav.addObject(owner);
    logger.info("Owner record accessed for {} {}", owner.getFirstName(), owner.getLastName());
    return mav;
}
```

Now we need to rebuild the application and run it again:

```cmd
./build.sh
```

Once the rebuild has completed we can then run the application again:

```cmd
./run.sh
```


## View Logs in Log Observer

From the left hand menu click on Log Observer. Click on Index and select o11y-workshop-XXX.splunkcloud.com (where XXX will be the realm you are running in). On the right hand side select petclinic-workshop and then click Apply. You should see log messages being reported.

Next click Add Filter and search for the field service_name and select the value <your host name>-petclinic-service and click = (include). You should now see only the log messages from your PetClinic application.


## Summary

This the end of the exercise and we have certainly covered a lot of ground. At this point you should have metrics, traces (APM & RUM), logs, database query performance and code profiling being reported into Splunk Observability Cloud. Congratulations!