# Lab 2.1: APM System Metrics and Profiling for Java

## Enable Always On Profiling and Metrics and Add Resource Attributes to Spans

To enable CPU and Memory Profiling on the application we can start the application by passing **splunk.profiler.enabled=true**, **splunk.profiler.memory.enabled=true** and for metrics pass **splunk.metrics.enabled=true**.

Make sure the application is stopped and update the startup command to enable metrics and profiling.

```cmd
sudo java \
-Dotel.service.name=$APP_NAME \
-Dotel.resource.attributes=deployment.environment=$ENV_NAME,version=0.970 \
-Dsplunk.profiler.enabled=true \
-Dsplunk.profiler.memory.enabled=true \
-Dsplunk.metrics.enabled=true \
-jar target/spring-petclinic-*.jar --spring.profiles.active=mysql
```

You can now visit the Splunk APM UI and examine the application components, traces, profiling, DB Query performance and metrics.

Once your validation is complete you can stop the application by pressing Ctrl-C.

## Next Step

[Go back to Main Page and Proceed to Lab 4](README.md)
