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

## Minikube

`minikube version`

`minikube start`

`kubectl version`

---

## Microk8s

`microk8s start`

`micork8s stop`

`microk8s status`

`microk8s inspect`

`microk8s kubectl cluster-info`

---


## View

### ...get all

`kubectl get all --all-namespaces`

### ...get deployments, pods, events,...

`kubectl get {deployments|pods|events|nodes} {-o wide}`

### ...describe deploymets, pods, events, ...

`kubectl describe {deployments|pods|events|nodes}`

### ...logs
`kubectl logs pods/<pod name>`

### ...cluster details

`kubectl cluster-info`

### ...cluster events

`kubectl get events`
### ...labels

`kubectl get deployment --show-labels`

### ...configuration

`kubectl config view`

---

### Executing commands in the container

`kubectl exec --stdin --tty <pod name> -- /bin/bash`

`kubectl exec -it kafka-client -- curl -X GET "http://url" -H "accept: */*"`

`kubectl exec $POD_NAME env`

`kubectl exec -ti $POD_NAME bash`

---

## Port fowarding

`kubectl port-forward  service/<service name> port:port --address 0.0.0.0`
`kubectl port-forward -n kube-system service/kubernetes-dashboard 10443:443`

## Deployments

### Rollout/restart a deployment

`kubectl rollout restart deployment/<deployment name>`


### Apply a deployment/service/ingress defined in a yaml file

`kubectl apply -f <file.yaml>`

---

## Ingress

`kubectl get ingress -a`
`kubectl describe ingress <name>`

---

## Kafka

### Read a Kafka topic

`kubectl exec -it kafka-client -- kafka-console-consumer --bootstrap-server <kafka instance>-kafka-headless:9092 --from-beginning --topic <topic>`

---

## Create a new deployment from an image

### ...a new deployment

`kubectl create deployment hello-node --image=k8s.gcr.io/echoserver:1.4`

### ...a new service

`kubectl expose deployment hello-node --type=LoadBalancer --port=8080`
`kubectl get services`

### .. a proxy

`kubectl proxy`

---

## Addons

### ...enable an addon using minikube

`minikube addons enable metrics-server`
`kubectl get pod,svc -n kube-system`

### ...disable an addon using minikube

`minikube addons disable metrics-server`

###  ...enable an addon using microk8s

`microk8s enable registry`

### ...disable an addon using minikube

`microkus disable registry`

--- 

## Images/containerd

### get a list of images as expected by all the pods

`kubectl get pods --all-namespaces -o jsonpath="{..image}" |tr -s '[[:space:]]' '\n' |sort |uniq -c`

### Logs

### ...conatinerd logs

`journalctl  -u snap.microk8s.daemon-containerd.service`

### ...registry pod logs

`kubectl get pods --all-namespaces`
`kubectl logs pods/registry-7cf58dcdcc-sld5l --namespace=container-registry`

### Containerd

### verify that containerd can pull an image from an external docker registry 
`microk8s ctr --debug images pull <registry>:<registry-port>/<image>:<tag>`

### list of endpoints on registry
`cat /var/snap/microk8s/current/args/containerd-template.toml | grep endpoint`

### list conateinerd's configured registries 
`cat /var/snap/microk8s/current/args/containerd-template.toml | grep endpoint`

### Docker Registry

### ...List images
`curl -X GET http://localhost:32000/v2/_catalog`
`curl -u user:password -X GET http://<host>:<port?/v2/_catalog`

### ...List tags 
`curl -X GET http://localhost:32000/v2/<image>/tags/list`
