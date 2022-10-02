---
title: WSL2 Kubernetes 환경에서 Windows 경로 Mount 하기
categories: [dev]
comments: true
tags: [kubernetes]
---

## 상황

WSL2 Kubernets 환경에서 Mount를 할 때, hostPath에 Windows 경로를 그대로 사용하였더니 mount가 제대로 안되었습니다.

<br/>

## 해결
Windows의 경로가 `D:\WorkSpaces\NodeJS\sample-app` 일 때,  <br/>
hostPath에는 `/run/desktop/mnt/host/d/WorkSpaces/NodeJS/sample-app` 와 같이 적어 줍니다.

```yaml
apiVersion: v1
kind: Pod
metadata:  
  name: node-app-dev 
spec:  
  containers: 
  - name: node-app-dev
    image: node:18.10.0
    command: ["bash"]
    args: ["-c", "while true; do echo hello; sleep 1000;done"]
    volumeMounts:
    - mountPath: /app
      name: mydir
  volumes:
  - name: mydir
    hostPath:
      path: /run/desktop/mnt/host/d/WorkSpaces/NodeJS/sample-app
      type: DirectoryOrCreate
```

<br/>

---
[참고]
- [https://stackoverflow.com/a/67053315](https://stackoverflow.com/a/67053315)