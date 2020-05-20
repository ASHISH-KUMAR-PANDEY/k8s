# Exam

### question 1
adhochttpd.dockerfile
```shell
FROM centos
MAINTAINER ashishkumarpandey2000@gmail.com
RUN yum install -y httpd
ENV x=app
EXPOSE 80
RUN mkdir /myapps
RUN mkdir /scripts
COPY beginner-html-site-styled /myapps/app1
COPY project-html-website /myapps/app2
COPY start.sh /scripts/start.sh
RUN chmod +x /scripts/start.sh
ENTRYPOINT ["/bin/bash","/scripts/start.sh"]
```

#### Git clone
```
git clone https://github.com/mdn/beginner-html-site-styled
git clone https://github.com/microsoft/project-html-website
```

#### start.sh
```shell
#!/bin/bash

if [ "$x" == "app1" ]
then
	cp -rf /mycode/webapp1/* /var/www/html/
	httpd -DFOREGROUND

elif [ "$x" == "app2" ]
then
	cp -rf /mycode/webapp2/* /var/www/html/
	httpd -DFOREGROUND
else
	echo "please select the correct env for app1 or app2" > /var/www/html/index.html
	httpd -DFOREGROUND
fi
```
#### Docker build command
```shell
docker build -f adhochttpd.dockerfile -t ashishpandey1504/may2020q1:v1 .
```
---------

### :two: Question 2

##### q2.yml
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    adhoc: ashishpandeyq2
  name: adhocpod1
spec:
  containers:
  - image: nginx
    name: adhocpod1
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
#### command
```shell
kubectl create -f q2.yaml
```

#### q2svcashishpandey.yml
```shell
apiVersion: v1
kind: Service
metadata:
  name: q2svcashishpandey
spec:
  type: NodePort
  selector:
    adhoc: ashishpandeyq2
  ports:
    - port: 80
      targetPort: 80
```
#### Command
```shell
kubectl create -f q2svcashishpandey.yml
```
--------
### Question 3
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    adhoc: ashishpandeyq3
  name: adhocpod2
spec:
  nodeSelector:
    kubernetes.io/hostname: ip-172-31-33-139.ec2.internal
  containers:
  - env:
    - name: x
      value: app2
    image: ashishpandey/may2020q1:v1
    name: adhocpod2
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
#### Create the pod
```
kubectl create -f q3.yaml
```

#### Create service file named "q3svcashishpandey.yml"
```
apiVersion: v1
kind: Service
metadata:
  name: q3svcrhythmbhiwani
spec:
  type: NodePort
  selector:
    adhoc: rhythmbhiwaniq3
  ports:
    - port: 80
      targetPort: 80
      nodePort: 31905
```
#### Create
```
kubectl create -f q3svcashishpandey.yml
```


## question 4
#### create a replicasets
##### Create file named "q4.yml"
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: adhocrsashishpandey4
  labels:
    app: adhocrsashishpandey4
spec:
  replicas: 1
  selector:
    matchLabels:
      adhoc: ashishpandeyq4 
  template:
    metadata:
      name: adhocpod4
      labels:
        adhoc: ashishpandeyq4
    spec:
      containers:
      - env:
        - name: x
          value: app2
        image: ashishpandey/may2020q1:v1
        imagePullPolicy: Always
        name: adhocpod4
        ports:
        - containerPort: 80
	
#### Create the replicaset
kubectl create -f q4.yml

#### Create file "q4svcashishpandey.yml"
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: adhocrsashishpandey4
  name: q4svcashishpandey
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    adhoc: ashishpandeyq4
  type: LoadBalancer
  
#### Create
kubectl create -f q4svcashishpandey.yml

## Question 5
##### Create file "q1dep1.yml"
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adhhocdepashishpandey5
  labels:
    adhoc: ashishpandeyq5
spec:
  replicas: 3
  selector:
    matchLabels:
      adhoc: ashishpandeyq5
  template:
    metadata:
      labels:
        adhoc: ashishpandeyq5
    spec:
       containers:
        - env:
          - name: x
            value: app2
          image: ashishpandey/may2020q1:v1
          imagePullPolicy: Always
          name: adhocpod2
          ports:
          - containerPort: 80
	  
#### Create
kubectl create -f q5dep1.yaml

#### Create file "q5svcashishpandey.yml"
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    adhoc: q5svcashishpandey
  name: q5svcashishpandey
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    adhoc: ashishpandeyq5
  type: LoadBalancer
  
#### Create
kubectl create -f q5svcashishpandey.yml
