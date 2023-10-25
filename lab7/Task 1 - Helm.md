# Task 1 - Helm

### Helm 을 이용하여 kubernetes 대시보드 패키지 설치
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
7. helm 으로 k8s 대시보드 설치
```
helm upgrade --install kubernetes-dashboard kubernetes-dashboard/kubernetes-dashboard --create-namespace --namespace kubernetes-dashboard
```
8. svc 노드포트로 변경 (type: NodePort)
```
kubectl edit -n kubernetes-dashboard svc kubernetes-dashboard
```

9. ClusterRoleBinding, ServiceAccount, Secret 확인 및 배포
```
cat kd-crb.yaml
```
```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
```
```
cat kd-sa.yaml
```
```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
```
```
cat kd-sa-secret.yaml
```
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: kd-sa-secret
  namespace: kubernetes-dashboard
  annotations:
    kubernetes.io/service-account.name: admin-user
type: kubernetes.io/service-account-token
```

```
cat kd-crb.yaml kd-sa.yaml kd-sa-secret.yaml
kubectl create -f kd-crb.yaml 
kubectl create -f kd-sa.yaml 
kubectl create -f kd-sa-secret.yaml
```

10. secret의 token을 확인하여 메모장에 저장
```
kubectl describe secrets -n kubernetes-dashboard 
```
11. worker node 1이나 2에서 Public IP확인
```
curl ifconfig.io
```

12. master node에서 대시보드 svc의 노드포트 넘버 확인
```
kubectl get svc -A
```

13. 웹브라우저로 접속
```
https://<WorkerNodePublic IP>:<NodePortNumber>
```
크롬 기준 : 하단 고급 버튼-> 안전하지않음으로 이동

14. 메모장에 저장한 토큰으로 로그인
