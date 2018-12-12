# django-postgres-minikube
Deploy Django app and Postgres on Minikube

# Requirements
+ [Install Docker](https://docs.docker.com/engine/installation/)
+ Minikube
+ Django 1.9.7

# Run command

## Build Django Image
#### Build Base Image: Django and Python3.6
	```
	$ cd django-postgres-minikube/devops
	$ docker build -t nhatthai/python3.6_django  .
	```

#### Build App:
	```
	$ cd backend
	$ docker build -t nhatthai/django-postgres .
	```

#### Push to Docker Hub
	```
	docker push nhatthai/django-postgres:latest
	```

### Start Minikube
	```
	$ minikube start
	```

### Create ConfigMap
    ```
    $cd devops/manifest
    $kubectl create -f configmap.yml
    ```

### Create Deployment Postgres
	```
	$ cd devops/manifest
	$ kubectl create -f postgres/deployment.yml
	```

### Create Service Postgres
	```
	$ cd devops/manifest
	$ kubectl create -f postgres/service.yml
	```

### Create Deployment
	```
	$ cd devops/manifest
	$ kubectl create -f django/deployment.yml
	```

### Create Service
	```
	$ cd devops/manifest
	$ kubectl create -f django/service.yml
	```

### Check Servive and run Command
	```
	$kubectl exec -it web-service /bin/bash

	```

#### Create Super User
	```
	python manage.py createsuperuser
	```

#### Run Server
	```
	root@web-service:#python manage.py runserver 0.0.0.0:8000
	```

### Ingress
#### Create Ingress
```
$ cd devops/manifest
$ kubectl create -f ingress.yml
```

#### Check Ingress status
```
kubectl describe ing
```

#### Start Ingress on Minkube
```
$ minikube addons enable ingress
```

#### Get IP
```
$ minikube ip
192.168.99.100
```

#### Add mysite.com into /etc/hosts
```
192.168.99.100 mysite.com
```

#### Check Django App
```
http://mysite.com/admin/
```

# Reference
[Writing your first Django App](https://docs.djangoproject.com/en/2.1/intro/tutorial01/)
