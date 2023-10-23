# Task 3 - Taint

### Taint 설정으로 Pod 할당 제어
#

1. Deployment 생성 yaml 확인 
```
cat taint-pod-dp.yaml
```

2. 생성
```
kubectl create -f taint-pod-dp.yaml
```

3. Deployment 에서 생성된 Pod의 배치 확인
```
kubectl get pod -o wide
```

4. Deployment 삭제
```
kubectl delete deploy --all
```

5. Worker 노드에 Taint 설정
```
kubectl taint nodes k8s-worker1 status=unstable:PreferNoSchedule
```

6. Worker 노드에 Taint 확인
```
kubectl describe node k8s-worker1 | grep Taint
```

7. 위에서 생성했던 Deployment를 다시 생성
```
kubectl create -f taint-pod-dp.yaml
```

8. Deployment 에서 생성된 Pod의 배치 확인
```
kubectl get pod -o wide
```

9. 다시 삭제
```
kubectl delete deploy --all
```

10. Taint 삭제
```
kubectl taint nodes k8s-worker1 status-
```

11. Worker 노드에 Taint 삭제 확인
```
kubectl describe node k8s-worker1 | grep Taint
```

12. 새로운 Taint 설정
```
kubectl taint nodes k8s-worker1 operation=upgrading:NoSchedule
```

13. Worker 노드에 Taint 확인
```
kubectl describe node k8s-worker1 | grep Taint
```

14. 위에서 생성했던 Deployment를 다시 생성
```
kubectl create -f taint-pod-dp.yaml
```

15. Deployment 에서 생성된 Pod의 배치 확인
```
kubectl get pod -o wide
```

16. 다시 삭제
```
kubectl delete deploy --all
```

17. Taint 삭제
```
kubectl taint nodes k8s-worker1 operation-
```

18. 위에서 생성했던 Deployment를 다시 생성
```
kubectl create -f taint-pod-dp.yaml
```

19. Deployment 에서 생성된 Pod의 배치 확인
```
kubectl get pod -o wide
```

20. Worker 노드에 Taint 설정
```
kubectl taint nodes k8s-worker1 performance=slow-disk:NoExecute
```

21. Worker 노드에 있던 Pod 가 이동 되는지 확인
```
kubectl get pod -o wide -w
```

22. Worker 노드에 설정한 Taint 삭제
```
kubectl taint nodes k8s-worker1 performance-
```

23. 다시 원상 복구 되는지 확인 (이동안함)
```
kubectl get pod -o wide
```

24. 리소스 삭제
```
kubectl delete deployment taint-deployment
```