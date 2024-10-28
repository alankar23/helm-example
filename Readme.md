# Helm

## Create Chart

```
helm create ChartName
```

## Install Chart
```
helm install <release-name> <chart-path> --dry-run --debug
helm install nginx . --values prod.yaml --dry-run --debug  
```

## Get Chart Values

```
helm show values grafana/loki-stack > loki.yaml
```