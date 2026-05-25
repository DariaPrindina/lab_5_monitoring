# Лабораторная №5. Мониторинг в Kubernetes

Подняла nginx в minikube, настроила сбор метрик через Prometheus и визуализацию в Grafana.

## Стек

- minikube
- Prometheus + Grafana (через helm-чарт `kube-prometheus-stack`)
- nginx как monitored-сервис

## Как запускала

```bash
minikube start

kubectl create namespace monitoring
kubectl apply -f k8s/app-deployment.yaml

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm install monitoring prometheus-community/kube-prometheus-stack -n monitoring
```

## Результат

### Поды

![pods](screenshots/1-pods-running.png)

Все компоненты поднялись в namespace `monitoring`.

### Prometheus — Targets

Prometheus подтягивает метрики с компонентов кластера. controller-manager, etcd, scheduler недоступны тк они не экспортируют метрики по умолчанию.

![targets-1](screenshots/4-prometheus-targets-1.png)

![targets-2](screenshots/5-prometheus-targets-2.png)

### Grafana — графики

Дашборд: **Kubernetes / Compute Resources / Node (Pods)**

**CPU Usage:**

![cpu](screenshots/2-grafana-cpu.png)

**Memory Usage:**

![memory](screenshots/3-grafana-memory.png)
