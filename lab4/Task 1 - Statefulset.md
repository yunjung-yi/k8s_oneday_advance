# Task 1 - Statefulset

### 두 컨트롤러를 생성해보며 stateful 과 stateless 비교
#

1. 실습 디렉토리 이동
```
cd ~/k8s_oneday_advance/lab4/yaml
```

2. yaml 확인
```
cat statefulset.yaml
```
```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: ss
spec:
  replicas: 1
  selector:
    matchLabels:
      type: db
  serviceName: "db"
  template:
    metadata:
      labels:
        type: db
    spec:
      containers:
      - name: container
        image: nginx
```
```
cat  replicaset.yaml
```
```yaml
kind: ReplicaSet
metadata:
  name: rs
spec:
  replicas: 1
  selector:
    matchLabels:
      type: web
  template:
    metadata:
      labels:
        type: web
    spec:
      containers:
      - name: container
        image: nginx
```

3. 각 리소스 생성
```
kubectl create -f statefulset.yaml
kubectl create -f replicaset.yaml
```

4. 생성한 리소스 확인
```
kubectl get rs
kubectl get sts
```

5.  터미널을 하나 더 오픈하여 모니터링용 터미널을 생성
```
watch -n 0.5 kubectl get pod
```

6. 위 3에서 생성한 두 컨트롤러의 replicas 를 5로 수정한 뒤 모니터링용 터미널 확인
```
kubectl scale sts ss --replicas=5

# 모니터링용 터미널 확인

kubectl scale rs rs --replicas=5

# 모니터링용 터미널 확인
```
7. 다시 두 컨틀롤러의 replicas를 0으로 수정한 뒤 확인
```
kubectl scale sts ss --replicas=0

# 모니터링용 터미널 확인

kubectl scale rs rs --replicas=0

# 모니터링용 터미널 확인
```

8. clear
```
kubectl delete statefulset,rs --all
```
