# Task 2 - NetworkPolicy

### NetworkPolicy 설정을 하여 IP 또는 Port 제한 제어를 설정한다.
#

1. 기존 Network 플러그인 삭제
```
kubectl delete -f https://raw.githubusercontent.com/wsjang619/k8s_course/master/lab1/yaml/flannel.yaml
```

2. calico network 플러그인 설치 yaml 다운로드
```
wget https://raw.githubusercontent.com/projectcalico/calico/v3.25.0/manifests/cailco.yaml
```
3. calico 설치
```
kubectl create -f calico.yaml
```

4. 정상동작 확인 (cailco 이름의 pod가 running)
```
kubectl get pod -A
```

5. 특정 Pod 만 접근하도록 설정하는 NetworkPolicy yaml 확인
```
cat somepod-np.yaml
```

6. pod yaml 확인
```
cat pod1.yaml
```

7. 생성
```
kubectl create -f pod1.yaml
``` 

8. 추가 Pod2,3 yaml 확인 및 생성
```
cat pod2.yaml
cat pod3.yaml
kubectl create -f pod2.yaml
kubectl create -f pod3.yaml
```

9. Pod1의 IP확인
```
kubectl get pod -o wide
```

10. Pod2로 접속
```
kubectl exec -it pod2 -- /bin/bash
```

11. curl 명령으로 pod1에 통신
```
curl <6에서 확인한 Pod1의 ip>:80
```

12. Pod2 에서 접속해제
```
exit
```

13. Pod3으로 접속
```
kubectl exec -it pod3 -- /bin/bash
```

14. curl 명령으로 pod1에 통신
```
curl <6에서 확인한 Pod1의 ip>:80
```

15. Pod3 에서 접속해제
```
exit
```

16. 특정 Namespace 의 Pod에서 접근을 허용하는 NetworkPolicy yaml 을 확인
```
cat ns-np.yaml
```

17. 생성 
```
kubectl create -f ns-np.yaml
```

18. ns 생성하는 yaml 확인
```
cat np-ns.yaml
```

19. 생성
```
kubectl create -f np-ns.yaml
```

20. np-ns 에 pod 생성
```
kubectl run np-ns-pod --image=nginx -n np-ns
```

21. np-ns 에 있는 np-ns-pod 에 접속
```
kubectl -n np-ns exec -it np-ns-pod -- /bin/bash
```

22. curl 명령을 이용하여 접근 시도
```
curl <6에서 확인한 Pod1의 ip>:80
```
접근이 잘되는 것을 확인.

23. np-ns-pod 에서 접속해제 후 pod2, pod3에서 접근시도

```
exit
```

```
kubectl exec -it pod2 -- /bin/bash
curl <6에서 확인한 Pod1의 ip>:80
```
<Ctrl + C>키로 취소
```
exit
```
```
kubectl exec -it pod3 -- /bin/bash
curl <6에서 확인한 Pod1의 ip>:80
```
<Ctrl + C>키로 취소
```
exit
```

접근이 안되는 것을 확인


22. 리소스삭제 
```
kubectl delete networkpolicy,pod --all
kubectl delete ns np-ns
```