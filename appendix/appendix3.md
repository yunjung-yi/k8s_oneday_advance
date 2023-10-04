# Appendix Lab 3. Monitoring tools

# helm 설치

```
curl -sSL https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash
```

# Repository 확인
```
helm repo add stable https://charts.helm.sh/stable
helm search repo stable
```

# helm bash 환경 설정
```
helm completion bash >> ~/.bash_completion
. /etc/profile.d/bash_completion.sh
. ~/.bash_completion
source <(helm completion bash)
```

# Prometheus, Grafana Repo 추가
```
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add grafana https://grafana.github.io/helm-charts
```

# 네임스페이스 생성 및 Helm 을 이용 Prometheus & Grafana 모니터링 도구 설치
```
helm install prometheus \
  prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace
```

#
