# Manual Instrumentation - Traces

One needs to understand the OpenTelemetry Tracing Signals to implement Manual tracing

this includes all the below steps in order

1) Tracing API - interface to add tracing instrumentation to application
2) Tracing SDK - contains trace providers, tracers, spanprocessors and span exporters - results in Spans
3) Trace providers creates a Tracer and Tracer connects to span processors, exportes thereby creating a span

Span
 -- name
 -- SpanContext
 -- Timestamp
 -- ...

Each span contains above set of information.

Resource -- resource in span is a set of static attributes that help us to identify the source that captured piece of telemetry

Semantic Conventions -- semconv -- Standardizations of SpanContext data using attributes instead of hardcoded strings

SpanAttributes -- Aside from static resources, spans can carry dynamic context using SpanAttributes

Context Propogation
- Span Context -- contextual metadata betweem adjacent operations which are seperated by logical boundary.
                -- Boundaries can be in-process or network

- Propogators -- propogators serialize the context, so it can traverse network along with the request
              -- opentelemetry.propogators API - provides extract and atttach functions to make this possible

- trace context -- Opentelemetry uses W3C standard to represent traceparent placeholder
            <spec-version>_<trace_id>_<parent_id>_<trace_flag>    -- format used for traceparent placeholder