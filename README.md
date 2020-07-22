# kubernetes notes

## Definitions

* **Cluster**: { Types: Master (coordinator ), Nodes (worker): { node processes }}
* **node**: VM or a physical computer that serves as a worker machine
* **deploying apps**: you tell the master to start the app containers, schedules containers to run the cluster's nodes. The communication is done by using the Kubernetes API, which the master exposes.
* **Deployments**: instructs Kubernetes how to create and update instances of your application. 


---

## Dashboard (UI)

`minikube dashboard`

---

## Misc minikube

`minikube version`

`minikube start`

`kubectl version`

---

## Tricks

### Get pod name, open a proxy in a differnt window, route to API

`POD_NAME=$(kubectl get pods -o go-template --template '{{range .items}}{{.metadata.name}}{{"\n"}}{{end}}')`

`kubectl proxy`

`curl http://localhost:8001/api/v1/namespaces/default/pods/$POD_NAME/proxy/`

### Executing commands in the container

`kubectl exec $POD_NAME env`

`kubectl exec -ti $POD_NAME bash`

---

## View

### ...deployments

`kubectl get deployments`

### ...pods

`kubectl get pods`

`kubectl describe pods`

### ...cluster details

`kubectl cluster-info`

### ...cluster events

`kubectl get events`

### ...container logs

`kubectl logs $POD_NAME`

### ...configuration

`kubectl config view`

### ...addon list

`minikube addons list`

---

## Create

### ...a new deployment

`kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4`

### ...a new service

`kubectl expose deployment hello-node --type=LoadBalancer --port=8080`
`kubectl get services`
`minikube service hello-node`


### .. a proxy

`kubectl proxy`


---

## Addons

### ...enable an addon

`minikube addons enable metrics-server`
`kubectl get pod,svc -n kube-system`

### ...disable an addon

`minikube addons disable metrics-server`

--- 

## Clean up

`kubectl delete service hello-node`
`kubectl delete deployment hello-node`

