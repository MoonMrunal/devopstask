# DevOpstask

#### 1. There should be 10 replicas running. (completed)
#### 2. The deployment should auto scale at average 50% CPU and 60% memory. (completed)
#### 3. Load test the application (can be done using JMeter) and include the test results in your submission. (incomplete)
#### 4. Use a custom docker image hosted on any docker registry called nodejs-test:latest (any region). (completed)
#### 5. The docker image which must be in private docker registry. (completed)
#### 6. Expose the app on port 3000 via Kubernetes LoadBalancer. (completed)
#### 7. Any change to the deployment should always ensure at least 7 replicas must always be running. (optional)
#### 8. Use automation wherever required (Ex- Jenkins for CI/CD, Terraform as Infra Provisioning). (completed)
#### 9. Any changes in the source code will not be accepted. (completed)
#### 10. All submissions must be accepted as GitHub repository. (completed)
#### 11. Please include a README file. (completed)

# Solution
## System : AmazonEC2 
## Tier :  3
## OS : Ubuntu 18.04
## CPU : 2 
## Memory : 2GB
## Technology stack used : Docker, DockerHub, Kubectl, Minikube,oracle vertual box, Putty, PuttyGen

##### For maximum privileges run 
```sudo su ```

#####Upgrade system
```apt-get upgrade ```

##### Install KubeCtl
##### Download latest release
```curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl" ``` 

##### Make the kubectl binary executable.
```chmod +x ./kubectl ``` 

##### Move the binary in to your PATH.
```sudo mv ./kubectl /usr/local/bin/kubectl ``` 

##### Test to ensure the version you installed is up-to-date:
```kubectl version --client ``` 

##### Install Docker
```apt-get install docker.io -y ``` 

##### Check version
```docker version ``` 

##### Install MiniKube
##### Install Minikube via direct download
```curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
  && chmod +x minikube ``` 

##### add the Minikube executable to your path:

```sudo mkdir -p /usr/local/bin/ ``` 
```sudo install minikube /usr/local/bin/ ``` 

##### Check version
```minikube version ``` 

##### Start minikube
```minikube start ``` 

##### If VM error occured executed
```minikube start -vm-driver=none ``` 

##### check minikube status
```minikube status ```

```sudo apt-get install -y conntrack ```

##### Minikube started

##### To deploy pod
##### To go inside minikube
```sudo -i ```

##### To create first container using kubectl command
```kubectl run hello-minikube --image=vaibhavdanao/node-hello-app --port=8080 ```

##### Check pods
```kubectl get pods ```

##### For 10 replica running
```kubectl scale deployment hello-minikube --replicas 10 ```

##### Task 1 completed

##### auto scale at average 50% CPU
```kubectl autoscale deployment hello-minikube --min=1 --max=4 --cpu-percent=50```

##### To get the available hpa in your cluster.
```kubectl get hpa```

##### auto scale at average 60% memory
``` vi memory.yaml```

#### Write to file
```
apiVersion: autoscaling/v2beta1
kind: HorizontalPodAutoscaler
metadata:
  name: hello-minikube
spec:
  maxReplicas: 4
  minReplicas: 1
  scaleTargetRef:
    apiVersion:
    kind: Deployment
    name: hello-minikube
  metrics:
  - type: Resource
    resource:
      name: memory
      targetAverageUtilization: 60
```

#### Apply changes
```kubectl apply -f memory.yaml```

#### List all resources
```kubectl get all```

##### Task 2 completed

##### Create docker file
```vi Dockerfile ```

##### Write to the file
```
FROM ubuntu

WORKDIR "/app"

RUN apt-get update \
 && apt-get dist-upgrade -y \
 && apt-get clean \
 && echo 'Finished installing dependencies'

RUN npm install --production

ENV NODE_ENV production
ENV PORT 3000

EXPOSE 3000

USER node

CMD ["npm", "start"]
```

##### To create image from dockerfile
```docker build -it nodejs ```

##### To check image
```docker images ```

##### Task 4 completed

##### Create Account on https://hub.docker.com/

##### login
```docker login ```

##### Push repository to docker hub
```docker push vaibhavdanao/node-hello-app ```

##### To make private repository
https://hub.docker.com/ -----> select node-hello-app repository ----> setting ----> make repository private

##### Task 5 completed

##### To set loadbalancer & expose on port 3000
```kubectl expose deployment hello-minikube -type=LoadBalancer --port=3000 ```

##### Check loadbalancer on port
```kubectl get services ```

##### Task 6 completed

##### Replication control for 7 replica in .yaml
```vi replication.yaml ```

##### Write to file 
```
apiVersion: v1
kind: ReplicationController
metadata:
  name: hello-minikube
spec:
  replicas: 7
  selector:
    app: hello-minikube
  template:
    metadata:
      name: hello-minikube
      labels:
        app: hello-minikube
    spec:
      containers:
      - name: hello-minikube
        image: hello-minikube
        ports:
        - containerPort: 3000
```
##### Run replication
```kubectl apply -f replication.yaml ```

##### Check rplications control
```kubectl describe replicationcontrollers/hello-minikube ```

##### Task 7 completed
##### Task 8 completed (optional)
##### Task 9, 10, 11 completed





