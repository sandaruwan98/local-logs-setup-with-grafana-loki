# Local Log Viewer Grafana and Loki

Tired of `docker logs -f` or grepping through log files? This spins up Promtail + Loki + Grafana so you can browse, search, and filter logs from all your local services in one place — similar to how GCP Cloud Logging works.

## Setup

```bash
docker-compose up -d
```

Then open Grafana at http://localhost:3000 — no login needed.

## Service Logs Dashboard

Head to **Dashboards → Service Logs**. You'll get:

- **Service** dropdown — filter by a specific service or view all at once
- **Severity** dropdown — narrow down to ERROR, WARN, INFO, etc.
- **Search** box — filter logs by anything: correlationId, a message keyword, logger name, thread, you name it. Leave it empty to see everything.
- A **log volume chart** at the top so you can spot spikes at a glance

## Need more freedom?

Use the **Explore** page (the compass icon on the left). You can write raw LogQL queries there — useful if you want to do things like cross-field filtering, custom time ranges, or just poke around.

A handy starting point in Explore:

```logql
{service="your-service-name"} | json | line_format "{{.message}}"
```

## How logs get here

Promtail watches Docker container logs and ships them to Loki automatically — nothing to configure per service.
