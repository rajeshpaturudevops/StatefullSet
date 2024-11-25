apiVersion: v1
kind: Service
metadata:
name: mysql
labels:
app: mysql
spec:
clusterIP: None
selector:
app: mysql
ports:
- name: tcp
protocol: TCP
port: 3306
---


apiVersion: apps/v1
kind: StatefulSet
metadata:
name: mysql
spec:
replicas: 1
serviceName: mysql
selector:
matchLabels:
app: mysql
template:
metadata:
labels:
app: mysql
spec:
volumes:
- name: task-pv-storage
persistentVolumeClaim:
claimName: task-pv-claim
containers:
- name: mysql
image: mysql:5.6
ports:
- name: tpc
protocol: TCP
containerPort: 3306
env:
- name: MYSQL_ROOT_PASSWORD
value: intelliqit
volumeMounts:
- name: task-pv-storage
mountPath: /var/lib/mysql
