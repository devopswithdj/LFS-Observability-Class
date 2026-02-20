export OTEL_TRACES_EXPORTER=console
export OTEL_METRICS_EXPORTER=none
export OTEL_LOGS_EXPORTER=none


export OTEL_TRACES_EXPORTER=otlp
export OTEL_COLLECTOR_HOST=localhost


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