Automatic instrumentation
--------------------------------

JAVA Application
----------------

1) Build the JAR file for application
mvn clean package

2) Install otel agent for java
wget https://github.com/open-telemetry/opentelemetry-java-instrumentation/releases/download/v2.8.0/opentelemetry-javaagent.jar

3) Setup Environment variables to OTEL
export OTEL_TRACES_EXPORTER=console
export OTEL_METRICS_EXPORTER=none
export OTEL_LOGS_EXPORTER=none

4) Run the application including otel agent 
java -javaagent:./opentelemetry-javaagent.jar -jar target/todobackend-0.0.1-SNAPSHOT.jar

5) Once Tested you can setup exporter endpoints like Jaeger UI
 -- below is command to run Jaeger all-in-one otel enabled container that runs UI on localhost:16686 
docker run -d --name jaeger -e COLLECTOR_OTLP_ENABLED=true -p 16686:16686 -p 14268:14268 -p 4317:4317 -p 4318:4318 jaegertracing/all-in-one

6) Update applications OTEL endpoints
- Stop the application using Ctrl^C, then after setting below env re run the step 4
export OTEL_TRACES_EXPORTER=otlp
export OTEL_COLLECTOR_HOST=localhost

7) Done - you can now access Jaeger UI showing traces for Java




export OTEL_TRACES_EXPORTER=console
export OTEL_METRICS_EXPORTER=none
export OTEL_LOGS_EXPORTER=none


export OTEL_TRACES_EXPORTER=otlp
export OTEL_COLLECTOR_HOST=localhost


Python Application
-------------------
# Setting up Otel for Python Application

pip install opentelemetry-distro opentelemetry-exporter-otlp

opentelemetry-bootstrap --action=install

pip uninstall opentelemetry-instrumentation-aws-lambda

export OTEL_LOGS_EXPORTER="none"
export OTEL_METRICS_EXPORTER="none"
export OTEL_TRACES_EXPORTER="otlp"
export OTEL_EXPORTER_OTLP_ENDPOINT="localhost:4317"
export OTEL_SERVICE_NAME=todoui-flask
export OTEL_EXPORTER_OTLP_INSECURE=true

Instead of running the application using the standard python or flask command, we now place a new script in front of it. This script is called opentelemetry-instrument and is a result of the just executed opentelemetry-bootstrap --action=install call.

Execute the call like this:

opentelemetry-instrument python app.py

The output will look exactly like the non-instrumented application, but in the background, the OpenTelemetry instrumentation will be active and sending telemetry data to the configured OTLP endpoint. You should see traces being exported to your OpenTelemetry Collector or backend that you have set up to receive OTLP data.