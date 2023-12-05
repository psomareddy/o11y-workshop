# Lab 4: Reduce MTTD/MTTR with Custom Attribution (Span Tagging)

## What are span attributes?

One of the reasons that OpenTelemetry is great at doing is that a lot of the common attributes you may find on a span are given standard names, so the systems receiving the data to visualize them don’t need to know the specifics of your system. This is really a superpower of OpenTelemetry, as it gives a level playing field for consumers of that information—meaning that you, as a developer, can forget about vendor-specific things.

Some examples of this are when you’re working with a HTTP API. The URL will be **http.url**, the method will be **http.method**, and the status code will be in **http.status_code**.

While auto instrumentation collects many system and application framework specific attributes, it is even more helpful for DevOps teams to add method parameters and local context as span tags.

You can use this to identify a spike in the throughput of a certain enterprise customer, or the user suffering the highest latency, or to pinpoint the database shard generating the most errors.  So you can see how adding appropriate span tags can reduce troubleshooting times (**MTTD**) considerably.

## How can we add more attributes for our PetClinic application?

Here we will add owner details to every owner information page (with an endpoint: /owners/{ownerId}).
If any issue happens with this page, we can immediately know which owner was affected.

### Code Instrumentation with Open Telemetry SDK

To manually instrument your application, add the open telemetry SDK dependency to the **pom.xml** file.

```xml
<dependency>
    <groupId>io.opentelemetry</groupId>
    <artifactId>opentelemetry-sdk</artifactId>
    <version>1.27.0</version>
</dependency>
```

Open the .../***src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java*** file.

```cmd
vi src/main/java/org/springframework/samples/petclinic/owner/OwnerController.java
```

Add this import statement (import statements are usually found at the top of the java source file)
```java
import io.opentelemetry.api.trace.Span;
```

Find the following method in the **OwnerController.java** file and add the code to get and add span tags. The added code basically accesses the current span (already created by auto instrumentation since spring-web is one of those frameworks supported by auto instrumentation) and adds some custom attributes to it.
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
    return mav;
}
```

Next, run the maven command to format and compile/build/package PetClinic

```cmd
./build.sh
```

Restart the PetClinic application.

```cmd
./run.sh
```

Go to the traces for the /owners/{owner-id} endpoint in your observability backend and find the custom attributes added.


