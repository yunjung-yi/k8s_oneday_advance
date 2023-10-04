# Task 1 - Helm

### Helm 을 이용하여 패키지 설치
#

1. 실습 디렉토리 이동
```
cd ~/k8s_oneday_advance/lab7/yaml
```

2. helm 다운로드
```
curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
```

3. 권한 수정
```
chmod 700 get_helm.sh
```

4. 실행
```
./get_helm.sh
```

5. search 명령어 테스트
```
helm search hub wordpress
```

6. 대시보드 repo 설치
```
helm repo add kubernetes-dashboard https://kubernetes.github.io/dashboard/
```

7. Helm 챠트 tgz 파일 생성
```
helm fetch stable/kubernetes-dashboard
```

8. 압축해제
```
tar -xzvf <압축파일명>
```

9. 익명 사용자를 위한 API 생성
```
cd kubernetes-dashboard/template
```

10. 아래내용으로 serviceaccount-custom.yaml 추가
```
vi serviceaccount-custom.yaml
```
```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
  - kind: ServiceAccount
    name: admin-user
    namespace: kubernetes-dashboard
---
kind: ClusterRole
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: kubernetes-dashboard-anonymous
rules:
- apiGroups: [""]
  resources: ["services/proxy"]  resourceNames: ["https:kubernetes-dashboard:https"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
  - nonResourceURLs: ["/ui", "/ui/*", "/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:https/proxy/*"]
  verbs: ["get", "list", "watch", "create", "update", "patch", "delete"]
---
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: kubernetes-dashboard-anonymous
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: kubernetes-dashboard-anonymous
subjects:
- kind: User
  name: system:anonymous
  namespace: kubernetes-dashboard
```

11. value.yaml 파일의 상위경로에서 helm install 실행
```
helm install -n kubernetes-dashboard kubernetes-dashboard kubernetes-dashboard
```

12. 외부에서 접근가능한 URL 조회
```
kubectl cluster-info -n kubernetes-dashboard
```

13. 워커노드의 Public ip 조회 
```
curl ifconfig.io
```

14. 웹브라우저에서 아래 주소로 접속
```
<12에서 확인한 URL에서 IP 부분을 13에서확인된 IP로 수정하여 접속>
```

15. 로그인하기 위한 토큰 확인
```
kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}”
```

16. 확인되는 토큰으로 접속