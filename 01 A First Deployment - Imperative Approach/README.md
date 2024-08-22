# First Deployment

using the imperative approach.

## Deploying via minikube

### Minikube?

Minikube is a tool that creates a `Kubernetes environment` on a local machine, such as a laptop or PC, to help developers and new users learn and experiment with Kubernetes: 

- **Lightweight**
Minikube is a lightweight implementation of Kubernetes that creates a `virtual machine (VM)` on your local machine and deploys a simple cluster with one node. 
- **Local**
Minikube provides a local Kubernetes environment that mimics the behavior of a production cluster, which can help developers test applications in a controlled environment and improve their Kubernetes skills. 
- **Easy to use**
Minikube is designed to be easy to learn and use, and you can start a Kubernetes cluster with a single command, minikube start.

### Lets follow the steps for deploying:

- Lets build the image for the application,
  ```
  docker build -t kub-first .  
  ```

- Now so as to push to docker hub, retag it as your repository name,
  ```
  docker tag kub-first-app hritikraj003/kub-first-app 
  ```
- Push the image to the dockerhub,
  ```
  docker push hritikraj003/kub-first-app 
  ```
- Now lets create a deployment resource in the kubernetes,
  ```
  kubectl create deployment first-app --image=hritikraj003/kub-first-app
  ```
   and now you can execute, `kubectl get deployments` you could deployment being created/
- To look at your cluster that is created cause of the deployment, you can execute,

  ```
  minikube dashboard
  ```
  this opens up a link on the browser, which shows the workload status something like this
   - Deployments
   - Pods
   - Replica sets
 
![image](https://github.com/user-attachments/assets/97fa468e-f431-44a4-9738-7fa2a283ab01)

 
 ## Updating Deployments

 for updating deployments, simply rebuilding the image and pushing to the dockerhub **wont Work**. 

 For that we need to retag the image with different version which may trigger the deployment after executing this,

 ```
 docker set deployment/<deployment_name> <existing_container_name>=<new image rebuilt from docker hub>
 ```
 so basically, we are informing the exisiting container to use the new built image ( with a new `version` ofcourse) and thus creating a new pod.

 ![image](https://github.com/user-attachments/assets/5d8d863b-6641-4e8e-a4ee-11bbaf2f9b45)

 and therefore, we also check the rollout status which tells us if the previous pod `terminated successfully` in order for the new pod to `run with the new built image`.


  
