# Task 1 - NodeName

### NodeName 설정으로 Pod 할당 제어
#

1. 실습 디렉토리 이동
```
cd ~/k8s_oneday_advance/lab5/yaml
```

2. nodename 설정을 이용한 rs yaml 확인
```
cat nodename-rs.yaml
```

3. yaml 생성
```
kubectl create -f nodename-rs.yaml
```

4. 위 결과로 생성된 Pod가 어떤 노드에 할당되었는지 확인 
```
kubectl get pod -o wide
```

5. 생성된 Replicaset 를 Scale Out
```
kubectl scale rs/nodename-rs --replicaset=5
```

6. 새롭게 생성된 Pod의 할당 확인 
```
kubectl get pod -o wide
```

7. 리소스 삭제
```
kubectl delete rs nodename-rs
```