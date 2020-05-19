# Helidon Quickstart SE Example

This project implements a simple Hello World REST service using Helidon SE.

## Build and run

With JDK 11
```bash
mvn package
java -jar target/helidon-quickstart-se.jar
```

## Exercise the application

```
curl -X GET http://localhost:8080/greet
{"message":"Hello World!"}

curl -X GET http://localhost:8080/greet/Joe
{"message":"Hello Joe!"}

curl -X PUT -H "Content-Type: application/json" -d '{"greeting" : "Hola"}' http://localhost:8080/greet/greeting

curl -X GET http://localhost:8080/greet/Jose
{"message":"Hola Jose!"}
```

## Try health and metrics

```
curl -s -X GET http://localhost:8080/health
{"outcome":"UP",...
. . .

# Prometheus Format
curl -s -X GET http://localhost:8080/metrics
# TYPE base:gc_g1_young_generation_count gauge
. . .

# JSON Format
curl -H 'Accept: application/json' -X GET http://localhost:8080/metrics
{"base":...
. . .

```

## Running in Google App Engine

Pick a [project ID](https://cloud.google.com/sdk/gcloud/reference/projects/create#PROJECT_ID) for your project and save it in an environment variable:

```bash
export YOUR_PROJECT_ID=[your_project_id]
```

Replace `[your_project_id]` with a string of characters that uniquely identifies your project.
For example, my-helidon-project-24

Go to the [Google App Engine Quickstart for Java 11:](https://cloud.google.com/appengine/docs/standard/java11/quickstart)
and complete the steps under "Before You Begin". Stop when you reach
"Download the Hello World app".

Verify your project:

```bash
gcloud projects describe $YOUR_PROJECT_ID
```

If you haven't already done it, build the Helidon example locally:

```bash
mvn package
```

Then deploy it to Google App Engine

```bash
gcloud app deploy target/gae-app.yaml
```

This will upload files, then deploy the service. 

You can view the application in your browser by running:

```bash
gcloud app browse
```

The browser will show:

```
Howdy
```

You can then append `/greet` to the URL in the browser and 
excercise the application as described earlier in this README.


## Some things to note

* This example responds to the
  [`PORT` environment variable](https://cloud.google.com/appengine/docs/standard/java11/runtime#be_sure_to_use_the_port_environment_variable)
  as provided by the Google App Engine runtime.
* By default Helidon packages your application as a "thin" jar file with runtime dependencies
  external to the jar (in `libs`). The jar file's `META-INF/MANIFEST.MF` has `Main-Class` and `Class-Path` 
  set appropriately.  Google App Engine [supports this packaging](https://cloud.google.com/appengine/docs/standard/java11/runtime#application_startup)
  as long as you use a custom entrypoint as is done in `target/gae-app.yaml`.

## Cleanup!

Make sure to cleanup your application as described in the  [Google App Engine Quickstart](https://cloud.google.com/appengine/docs/standard/java11/quickstart#clean-up).

