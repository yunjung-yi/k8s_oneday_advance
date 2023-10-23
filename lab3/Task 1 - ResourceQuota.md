# Task 2 - ResourseQuota

### Namespace 리소스를 제한
#
실습 디렉토리 이동
```
cd ~/k8s_oneday_advance/lab3/yaml
```

1. yaml 확인
```
cat rq-ns.yaml rq1.yaml
```

2. 리소스 생성
```
kubectl create -f rq-ns.yaml
kubectl create -f rq1.yaml
```

3. 생성 확인
```
kubectl get ns
kubectl get resourcequota -n rq-ns
kubectl get quota -n rq-ns
```

4. 상세 정보 조회
```
kubectl describe quota -n rq-ns
```

5. pod 생성 yaml 확인
```
cat rq1-pod.yaml
```

6. 리소스 생성
```
kubectl create -f rq1-pod.yaml
```
`rq 제한에 위배되어 생성되지 않음을 확인`


7. 두번째 Pod 생성 yaml 확인
```
cat rq2-pod.yaml
```

8. 리소스 생성
```
kubectl create -f rq2-pod.yaml
```
`정상 생성을 확인`

9. ResourceQuota 확인
```
kubectl describe quota -n rq-ns
```

10. rq1 삭제
```
kubectl delete quota rq1 -n rq-ns
```

11. 두번째 ResourceQuota yaml 확인
```
cat rq2.yaml
```

12. rq2 생성
```
kubectl create -f rq2.yaml
```

13. ResourceQuota 확인
```
kubectl describe quota -n rq-ns
```

14. pod 추가 생성 시도
```
kubectl create deploy rq-dp --image=nginx --replicas=3 -n rq-ns
```

15. pod 확인
```
kubectl get pod -n rq-ns
```

16. 세부 정보 확인 
```
kubectl describe deploy rq-dp -n rq-ns
kubectl describe rs rq-dp -n rq-ns
```


17. ns 삭제
```
kubectl delete ns rq-ns
```