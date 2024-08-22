# Imperative vs Declarative Kubernetes Usage

## Declarative

- A config file is defined and app;ied to chage the desired state.
- Comparable to using docker compose in the yaml file
  
```
kubectl apply -f config.yaml
```

so we can just create a `deployment.yaml` file 

``` yaml
apiVersion: apps/v1
kind: Deployment 
metadata: 
  name: second-app-deployment
spec: 
  replicas: 1
  selector:
    matchLabels:
      app: second-app
      tier: backend

  template:
    metadata:
      labels:
        app: second-app
        tier: backend
    spec: 
      containers: 
        - name: second-node
          image: hritikraj003/kub-first-app:2
```
Then in the terminal, we can just execute ` kubectl apply -f=deployment.yaml`, and voila! Your deployment is created but we are not done yet, we will have to create `service.yaml` file as well,

```yaml
apiVersion: v1
kind: Service
metadata:
  name: backend
spec:
  selector:
    app: second-app
  ports:
    - protocol: 'TCP'
      port: 80
      targetPort: 8080
  type: LoadBalancer
```

and in the same fashion, execute the `kubectl apply -f=service.yaml` command to create the service.


## Important terms to look for:

Below is a simplified explanation of each term in a Kubernetes `deployment.yaml` file:

### 1. **Selector**
   - **What it does:** It defines the criteria for selecting the pods that the deployment will manage.
   - **In simple terms:** Think of it as a filter that tells Kubernetes which pods this deployment is responsible for based on labels.

### 2. **Template**
   - **What it does:** It defines the blueprint for creating pods. The template specifies what the pods should look like.
   - **In simple terms:** It’s like a cookie cutter, where the `template` defines the shape (specs) of the cookie (pod) that will be made.

### 3. **Metadata**
   - **What it does:** Contains metadata like name, labels, and annotations for objects like deployments and pods.
   - **In simple terms:** It’s the ID card for a Kubernetes object, containing details like name and other identifiers.

### 4. **Spec**
   - **What it does:** Defines the desired state of the Kubernetes object, like how many replicas, what containers, etc.
   - **In simple terms:** It’s the recipe that tells Kubernetes how to create and manage the object, whether it’s a pod, deployment, or service.

### 5. **Labels**
   - **What it does:** Key-value pairs used for organizing and selecting objects in Kubernetes.
   - **In simple terms:** These are tags or labels you put on objects (like sticky notes) so you can easily find and group them.

### 6. **matchLabels**
   - **What it does:** Part of the selector, `matchLabels` specifies which labels should be matched to select the pods.
   - **In simple terms:** It’s like telling Kubernetes, "I want you to manage all pods that have these specific labels."

### 7. **matchExpressions**
   - **What it does:** Another way to select pods, using more complex conditions than `matchLabels`.
   - **In simple terms:** It’s a more advanced filter that allows you to select pods based on several criteria, like "has this label" or "does not have this label."

### 8. **Group**
   - **What it does:** Refers to API groups in Kubernetes that categorize the resources and provide versioning (like apps/v1).
   - **In simple terms:** It’s like a folder in Kubernetes where similar kinds of objects are grouped, such as deployments and replicas.

### 9. **Image Pull Policy**
   - **What it does:** Controls when Kubernetes should pull the Docker image for the container (e.g., always, if not present, never).
   - **In simple terms:** It’s the rule that decides whether Kubernetes should download the container image every time it starts a pod or only when needed.

### 10. **Liveness Probe**
   - **What it does:** A mechanism to check if a container inside a pod is still running correctly. If it fails, Kubernetes will restart the container.
   - **In simple terms:** It’s like a health check-up to ensure the app inside the pod is alive and functioning. If not, it will be restarted.

These components work together in a Kubernetes configuration file to define how your application should be deployed, managed, and maintained by the Kubernetes cluster.
