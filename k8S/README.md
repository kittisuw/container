# Kubernetes(K8S) Scheduling
## kind: Pod
หน่วยย่อยที่สุดของ K8S
```
```
## kind: ReplicaSet

กำหนดจำนวน Pod
```
```
## kind: Deployment,StatefulSets,DaemonSet
เกำหนดค่าต่าง ๆ ได้ดังนี้
- spec.replicas : จำนวน pod เช่นกำนหนดให้ run 5 pod
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-deployment
spec:
  replicas: 5
...
```
- spec.spec.affinity.nodeAffinity: กำหนาดเงื่อนไขการ run pod โดยถ้าตรงเงื่อนไขของ Pod จะได้คะแนนตาม weight ที่กำหนดให้ เข่นบอกว่า ถ้าตรงกับ hostname ให้ weight 100%
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app.kubernetes.io/component: query-layer
    app.kubernetes.io/instance: thanos-query
    app.kubernetes.io/name: thanos-query
    app.kubernetes.io/version: v0.12.0
  name: thanos-query
  namespace: thanos
spec:
  replicas: 1
  selector:
    matchLabels:
      app.kubernetes.io/component: query-layer
      app.kubernetes.io/instance: thanos-query
      app.kubernetes.io/name: thanos-query
  template:
    metadata:
      labels:
        app.kubernetes.io/component: query-layer
        app.kubernetes.io/instance: thanos-query
        app.kubernetes.io/name: thanos-query
        app.kubernetes.io/version: v0.12.0
    spec:
      affinity:
        podAntiAffinity:
          preferredDuringSchedulingIgnoredDuringExecution:
          - podAffinityTerm:
              labelSelector:
                matchExpressions:
                - key: app.kubernetes.io/name
                  operator: In
                  values:
                  - thanos-query
              namespaces:
              - thanos
              topologyKey: kubernetes.io/hostname
            weight: 100
```
- spec.containers[].imagePullPolicy: xxxx
- requests.memory:
- requests.cpu: หน่วยเป็น vcpu ต่ำสุดที่ set ได้คือ 0.1
- limits.memory:
- limits.cpu: หน่วยเป็น vcpu ต่ำสุดที่ set ได้คือ 0.1
```
```
## kind: Service
กำหนดวิธีการติดต่อกับ pod จากภายในและนอก cluster






Reference:
https://life.wongnai.com/how-kubernetes-schedule-pods-352a7bb0eb10
