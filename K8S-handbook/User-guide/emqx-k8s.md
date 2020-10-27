apiVersion: v1
kind: Service
metadata:
  name: emqx-mqtt
spec:
  type: NodePort
  selector:
    app: emqx
  ports:
    - port: 1883
      targetPort: 1883
      # Optional field
      nodePort: 30008

[root@centos7 k3s]# cat emqx.yaml
---
apiVersion: v1
kind: ServiceAccount
metadata:
  namespace: default
  name: emqx
---

apiVersion: rbac.authorization.k8s.io/v1beta1
kind: Role
metadata:
  namespace: default
  name: emqx
rules:
- apiGroups: [""]
  resources: ["endpoints"]
  verbs: ["get","watch", "list"]

---

apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  namespace: default
  name: emqx
subjects:
- kind: ServiceAccount
  name: emqx
  namespace: default
roleRef:
  kind: Role
  name: emqx
  apiGroup: rbac.authorization.k8s.io

---
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: emqx-pvc
  labels:
    app: emqx
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 20Gi

---

apiVersion: apps/v1
kind: Deployment
metadata:
  name: emqx-deployment
  labels:
    app: emqx
spec:
  replicas: 1
  selector:
    matchLabels:
      app: emqx
  template:
    metadata:
      labels:
        app: emqx
    spec:
      serviceAccountName: emqx
      nodeSelector:
        kubernetes.io/arch: amd64
      volumes:
      - name: emqx-data
        persistentVolumeClaim:
          claimName: emqx-pvc
      containers:
      - name: emqx
        image: emqx/emqx:v4.1-rc.1
        ports:
        - name: mqtt
          containerPort: 1883
        - name: mqttssl
          containerPort: 8883
        - name: mgmt
          containerPort: 8081
        - name: ws
          containerPort: 8083
        - name: wss
          containerPort: 8084
        - name: dashboard
          containerPort: 18083
        volumeMounts:
        - name: emqx-data
          mountPath: "/opt/emqx/data/mnesia"

---
apiVersion: v1
kind: Service
metadata:
  name: emqx-service
spec:
  selector:
    app: emqx
  ports:
    - name: mqtt
      port: 1883
      protocol: TCP
      targetPort: mqtt
    - name: mqttssl
      port: 8883
      protocol: TCP
      targetPort: mqttssl
    - name: mgmt
      port: 8081
      protocol: TCP
      targetPort: mgmt
    - name: ws
      port: 8083
      protocol: TCP
      targetPort: ws
    - name: wss
      port: 8084
      protocol: TCP
      targetPort: wss
    - name: dashboard
      port: 18083
      protocol: TCP
      targetPort: dashboard

