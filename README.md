This repository contains a small Grafana dashboard using metrics collected by the OpenTelemetry [docker stats receiver].

<img width="1248" height="727" alt="image" src="https://github.com/user-attachments/assets/d1299a59-ac9a-47fe-a220-4d1a0adc7acd" />

See the doc of [docker stats receiver] about how to configure your OpenTelemetry collector to use it.

## OpenTelemetry ingestor

If you use OpenTelemetry ingestor for your metrics storage, you need to promote the `container.name` attribute to a label.
Grafana cloud or the [otel-lgtm docker image] do this by default.

### Prometheus

```yml
otlp:
  keep_identifying_resource_attributes: true
  promote_resource_attributes:
    - container.name
```

There are a bunch of recommended labels, just `container.name` is needed for this dashboard. More details [here].

### Mimir

We do the [equivalent configuration].

```yml
limits:
  otel_keep_identifying_resource_attributes: true
  # You can add other attibutes if you want, they must be comma separated without space
  promote_otel_resource_attributes: "container.name"
```


[docker stats receiver]: https://github.com/open-telemetry/opentelemetry-collector-contrib/blob/ef72f9e9904f887122072f787391c7a09573dd3f/receiver/dockerstatsreceiver/README.md
[equivalent configuration]: https://grafana.com/docs/mimir/latest/configure/configure-otel-collector/#work-with-default-opentelemetry-labels
[here]: https://prometheus.io/docs/guides/opentelemetry/#promoting-resource-attributes
[otel-lgtm docker image]: https://github.com/grafana/docker-otel-lgtm/tree/main
