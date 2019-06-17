# 概要
[easy](../)はとりあえず、終了させて小さな達成感を得ることが目的です。ここでは、それらで扱っていたものに関しての説明を行っていきます。

## [demo.yaml](../demo.yaml)

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  name: demo
  labels:
    app: demo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: demo
  template:
    metadata:
      labels:
        app: demo
    spec:
      containers:
        - name: demo
          image: gtb_example01:latest
          ports:
            - containerPort: 80
---
apiVersion: v1
kind: Service
metadata:
  name: demo
  labels:
    app: demo
spec:
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 80
    nodePort: 30080
  selector:
    app: demo
  type: LoadBalancer
```
