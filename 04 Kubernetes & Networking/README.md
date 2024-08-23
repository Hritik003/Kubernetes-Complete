# Kubernetes and Networking

- We are gonna look at services
- Pod-Internal communication
- pod-to-pod communication

In this application, we have three `api's` where we dont have any db but we will create  `dummy` storage and make these apis talk with each other. 

<img width="1096" alt="image" src="https://github.com/user-attachments/assets/3374a002-b510-46dd-b96d-0a7bb40849f9">

## Lets dive in!

So at first, we create a deployment for **users-api**,

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: users-deployment
spec:
  replicas: 1
  selector:
    matchLabels: 
      app: users
  template: 
    metadata:
      labels:
        app: users
    spec: 
      containers:
        - name: users
          image: hritikraj003/kub-net
```
'but to access it from the outside world eg., Postman or any client, we will be needing a service. So lets create a `users-service.yaml `

```yaml

```
