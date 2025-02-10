StatefulSet is a namespace scoped Kubernetes API resource type that is used to manage Stateful workloads (See Definition of Stateful Workload). A StatefulSet manifest is shown in the example below. Note that in the below manifest, a Service is defined alongside the StatefulSet and the reason for having it is explained in Stable Network Identity of Stateful Pods.

apiVersion: v1
kind: Service
metadata:
  labels:
    app: demo-nginx-sf
  name: demo-nginx-stateful
  namespace: test
spec:
  clusterIP: None
  ports:
  - port: 8080
    protocol: TCP
    targetPort: 8080
  selector:
    app: demo-nginx-sf
  type: ClusterIP
 
---
 
apiVersion: apps/v1
kind: StatefulSet
metadata:
  labels:
    app: demo-nginx-sf
  name: demo-nginx-stateful
  namespace: test
spec:
  podManagementPolicy: OrderedReady
  replicas: 2
  selector:
    matchLabels:
      app: demo-nginx-sf
  serviceName: demo-nginx-stateful
  template:
    metadata:
      labels:
        app: demo-nginx-sf
    spec:
      containers:
      - image: container-registry.alm.fkcloud.in/alm/bitnami/nginx:1.18
        imagePullPolicy: IfNotPresent
        name: nginx-app-container
        ports:
        - containerPort: 80
          protocol: TCP
        resources:
          limits:
            cpu: 20m
            memory: 64Mi
          requests:
            cpu: 20m
            memory: 64Mi
        securityContext:
          runAsNonRoot: true
          runAsUser: 1001
          runAsGroup: 2001
          fsGroup: 2001
        volumeMounts:
        - mountPath: /data/store
          name: nginx-sf-pvc
  volumeClaimTemplates:
  - apiVersion: v1
    kind: PersistentVolumeClaim
    metadata:
      name: nginx-sf-pvc
    spec:
      accessModes:
      - ReadWriteOnce
      resources:
        requests:
          storage: 1Gi
      storageClassName: jbod-ssd-0
      volumeMode: Filesystem

In case of stateless applications, Users do not run create Pods individually, but create objects of resource type Deployment. Each Deployment object in turn owns objects of type ReplicaSet, that governs the running of required number of identical replica Pods that share the same spec.

Similarly in case of stateful applications, Users should create objects of resource type StatefulSet that governs the running of required number of identical replica Pods (preferrably using persistentVolumeClaim type volumes). However, there are some major differences between Deployments and StatefulSets as mentioned below.
