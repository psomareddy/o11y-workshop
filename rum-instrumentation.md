# Lab 3: Real User Monitoring with Browser Instrumentation

## Generate RUM Instrumentation Javascript 

For the Real User Monitoring (RUM) instrumentation, we will add
the [Open Telemetry Javascript](https://github.com/signalfx/splunk-otel-js-web) snippet in the GUI pages. 

Choose one of following ways to generate the JS snippet

1. We can use the wizard again **Data Management → Add Integration → Monitor User Experience tab → Browser Instrumentation**.

Select the preconfigured **RUM ACCESS TOKEN** from the dropdown, click **Next**. Enter **App name** and **Environment** with the values already chosen in the previous lab.

The wizard will then show a snipped of HTML code that needs to be place at the top at the pages in the **&lt;head&gt;** section. 
Copy the generated JS code snippet.

OR

2. Alternatively, copy and edit the JS snippet below accordingly. You need to
Update values for **&lt;rumAuth&gt;**, **&lt;app&gt;** and **&lt;environment&gt;** names.

```cmd
<script src="https://cdn.signalfx.com/o11y-gdi-rum/latest/splunk-otel-web.js" crossorigin="anonymous"></script>
<script>
SplunkRum.init({
    beaconUrl: "https://rum-ingest.us1.signalfx.com/v1/rum",
    rumAuth: "REPLACE_ME",
    app: "REPLACE_ME",
    environment: "REPLACE_ME"
    });
</script>
```

> **_NOTE:_**  The **application name** and **environment** must match exactly the values for **OTEL_SERVICE_NAME** and **deployment.environment** we used in the APM configuration. 
> These values are used by the Splunk Observability Cloud to correlate Frontend User Sessions to Backend APM Traces.

## Enable RUM

The Spring PetClinic application uses a single HTML page as the "layout" page, that is reused across all pages of the
application. This is the perfect location to insert the Splunk RUM Instrumentation Library as it will be loaded in all
pages automatically

Let’s then edit the layout page:

```cmd
vi src/main/resources/templates/fragments/layout.html
```

So, insert the snippet we generated above in the <mark style="background-color: #FDFDC9">&lt;head&gt;</mark> section of the layout page. 

Now we need to rebuild the application and run it again:

## Rebuild PetClinic

Run the <mark style="background-color: #FDFDC9">maven</mark> command to compile/build/package PetClinic:

```cmd
./build.sh
```

```cmd
./run.sh
```

Then let’s visit the application using a browser to generate real-user traffic http://&lt;VM_IP_ADDRESS&gt;:8080, now we
should see RUM traces being reported.

Let’s visit RUM to see the traces and metrics by clicking on **Hamburger Menu → RUM**.

Click on the **App Name** to visit the frontend metrics page for that application. The in any chart (for example: **Page Load/Route Change Duration** chart), click on any URL of interest to go **Tag Spotlight** page filtered for that URL. You can then click on the **User Sessions** tab to visit the frontend traces for that application. Play around with the filters to filter down to the subset of traces you want to focus on. Then click on a particular User Session belonging to a specific user to examine it in more detail.

When you drill down into a RUM trace you will see a link to APM in the spans. Clicking on the trace ID will take you to
the corresponding APM trace for the current RUM trace.

## Next Step

[Go back to Main Page and Proceed to Lab 5](README.md)