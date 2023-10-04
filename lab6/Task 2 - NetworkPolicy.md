# Task 2 - NetworkPolicy

### NetworkPolicy 설정을 하여 IP 또는 Port 제한 제어를 설정한다.
#

1. yaml 확인
```
cat pod1.yaml
```

2. 생성
```
kubectl create -f pod1.yaml
```

3. 특정 Pod 만 접근하도록 설정하는 NetworkPolicy yaml 확인
```
cat somepod-np.yaml
```

4. 생성
```
kubectl create -f somepod-np.yaml
```

5. 추가 Pod2,3 yaml 확인 및 생성
```
cat pod2.yaml
cat pod3.yaml
kubectl create -f pod2.yaml
kubectl create -f pod3.yaml
```

6. Pod1의 IP확인
```
kubectl get pod -o wide
```

7. Pod2로 접속
```
kubectl exec -it pod2 -- /bin/bash
```

8. curl 명령으로 pod1에 통신
```
curl <6에서 확인한 Pod1의 ip>:80
```

9. Pod2 에서 접속해제
```
exit
```

10. Pod3으로 접속
```
kubectl exec -it pod3 -- /bin/bash
```

11. curl 명령으로 pod1에 통신
```
curl <6에서 확인한 Pod1의 ip>:80
```

12. Pod3 에서 접속해제
```
exit
```

13. 특정 Namespace 의 Pod에서 접근을 허용하는 NetworkPolicy yaml 을 확인
```
cat ns-np.yaml
```

14. 생성 
```
kubectl create -f ns-np.yaml
```

15. ns와 Pod를 생성하는 yaml 확인
```
cat pod4.yaml
```

16. 생성
```
kubectl create -f pod4.yaml
```

17. ns4 에 있는 pod4 에 접속
```
kubectl -n ns4 exec -it pod4 -- /bin/bash
```

18. curl 명령을 이용하여 접근 시도
```
curl <6에서 확인한 Pod1의 ip>:80
```


19. 리소스삭제 
```
kubectl delete -f ns-np.yaml
```