# Open Telemetry Instrumentation Workshop with PetClinic Sample Application

The goal is to walk through the basic steps to configure the following components of the Splunk Observability platform:

- Infrastructure Monitoring (IM)
- Zero Configuration Auto Instrumentation for Java (APM)
- Database Query Performance
- Splunk Real User Monitoring (RUM)
- RUM spans to APM spans
- Custom Instrumentation

We will also show the steps about how to clone (download) a sample Java application (Spring PetClinic), as well as how to compile, package and run the application.

Once the application is up and running, we will instantly start seeing metrics and traces via the Zero Configuration Auto Instrumentation for Java that will be used by the Splunk APM product.

After that, we will instrument the PetClinic’s end user interface (HTML pages rendered by the application) with the Splunk OpenTelemetry Javascript Libraries (RUM) that will generate RUM traces around all the individual clicks and page loads executed by an end user.

Finally, we will demonstrate how to add custom span tags to some traces that will aid in troubleshooting issues reducing MTTD/MTTR.

## [Lab 1: Install the Open Telemetry Collector](install-otel-collector.md)

This lab walks you through an installation of Open Telemetry Collector on a Linux host. The Open Telemetry collector is
a proxy used to receive, process, and export telemetry data. It supports receiving data in several formats and can be
configured to send data to multiple backends. It is frequently conveniently bundled with a receiver to report host (
infrastructure) metrics and the ability to automatically download and apply auto instrumentation to Java, .Net
applications running on the same host.

## [Lab 2: APM Zero Configuration Auto Instrumentation for Java](sample-app-setup.md)

In this lab, we will walk through the Java auto instrumentation for Pet Clinic sample application. We will see how APM
traces are captured in full fidelity including calls to database.

## [Lab 3: Real User Monitoring Instrumentation for Browser](rum-instrumentation.md)

In this lab, we will continue with the instrumentation for Pet Clinic sample application. We will add Open Telemetry
Javascript instrumentation to monitor the frontend (GUI) pages and see how we can track entire user journeys in full fidelity and how
they are automatically connected to APM traces for end to end visibility.

## [Lab 4: Reduce MTTD with Custom Attribution (Span Tagging)](custom-tagging.md)

This lab walks through adding custom span tags to the petclinic application. You can use this to identify a spike in the
throughput of a certain enterprise customer, or the user suffering the highest latency, or to pinpoint the database
shard generating the most errors, which will demonstrate how adding appropriate span tags can reduce troubleshooting
times (MTTD) considerably.

## [Lab 5: Configure application logs and forward them with Otel Collector](log-instrumentation.md)

In this lab, we will write logs to a file in the filesystem and configure the Splunk OpenTelemetry Collector to read (tail) that log file and report the information to the Splunk Observability Platform.

