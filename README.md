# my-app-observabilitystack


#Full observability stack — Prometheus + Grafana on EKS, Fluentbit → CloudWatch, with metrics instrumented inside the Node.js app.

Ran 2 commandsRan 2 commands
#STEP 1 — Instrument the Node.js App

#STEP 2 — Prometheus ConfigCreated

#STEP 3 — Fluentbit → CloudWatchCreated

#STEP 4 — Grafana DashboardGrafana dashboard ConfigMap for myappGrafana dashboard ConfigMap for myapp

#STEP 5 — Helm chart annotation + install scriptRan , created a file, read a fileHere's what was added. Download the updated cicd-pipeline.zip.

What's new
```
app/app.js — instrumented with prom-client:

/metrics endpoint exposes Prometheus-format metrics
Tracks http_requests_total, http_request_duration_seconds, http_active_connections per route/method/status
Default Node.js metrics: heap memory, CPU, event loop lag — automatically collected

observability/prometheus/

values.yaml — installs Prometheus + Grafana + AlertManager via kube-prometheus-stack Helm chart
service-monitor.yaml — tells Prometheus to auto-discover and scrape /metrics every 15s

observability/alertmanager/

alert-rules.yaml — fires alerts for: app down, >5% error rate, p95 latency >1s, crash looping, memory >400Mi
alertmanager-config.yaml — routes alerts to Slack (#alerts for warnings, #alerts-critical for critical)

observability/grafana/

dashboard-configmap.yaml — pre-built dashboard with 8 panels: request rate, error %, p50/p95/p99 latency, memory, pod restarts — auto-loads into Grafana

observability/fluentbit/

values.yaml — DaemonSet that ships every pod log from production namespace → CloudWatch log group /eks/myapp-cluster/production
iam-addon.tf — Terraform for CloudWatch log group, metric filter on 5xx errors, SNS alarm → email

To install everything after EKS is up:
bashchmod +x observability/install.sh
./observability/install.sh
3 things to replace before running:
```
```
observability/alertmanager/alertmanager-config.yaml → your Slack webhook URL
observability/fluentbit/iam-addon.tf → your email for SNS alerts
observability/fluentbit/values.yaml → your AWS account ID in the IRSA annotation (optional)
Cicd pipelineZIP DownloadAppJS DownloadInstallSH DownloadService monitorYAML DownloadAlert rulesYAML DownloadDashboard configmapYAML DownloadValuesYAML DownloadDownload allYou 
```
