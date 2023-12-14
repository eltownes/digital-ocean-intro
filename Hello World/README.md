## Digital Ocean

`brew install doctl`

Init:
* `doctl auth init --context test1`
* `doctl auth list`
* `doctl auth switch --context test1`


Verify:
* `doctl account get`


Test:
* `doctl compute droplet create --region sfo2 --image ubuntu-23-10-x64 --size s-1vcpu-1gb test1-droplet`
* `doctl compute image list`
* `doctl compute droplet delete <DROPLET-ID>
  `

## Kubernetes

`brew install kubectl`

`kubectl version --client`


## Build app



## Docker image

* create `Dockerfile`
* `docker build -t my-python-app .`
* `docker images`
* `docker run -ti -p 80:80 my-python-app`


## Upload to registry

* `doctl registry login`
* `doctl registry create registry.digitalocean.com/kats-hello-world`
* `docker tag my-python-app registry.digitalocean.com/kats-hello-world/my-python-app`
* `docker push registry.digitalocean.com/kats-hello-world/my-python-app`

run it
* `docker run -p 80:80 registry.digitalocean.com/kats-hello-world/my-python-app`


## Cluster

Create
* `doctl kubernetes cluster create kats-cluster1 --tag hello-tutorial --auto-upgrade=true --node-pool "name=mypool;count=2;auto-scale=true;min-nodes=1;max-nodes=3;tag=hello-tutorial"`
* cluster credentials to kubeconfig file found in "/Users/erictownes/.kube/config"


Authorize k8s to use registry - generate manifest and pipe to kubectl
* `doctl registry kubernetes-manifest | kubectl apply -f -`

The secret (registry creds) is uploaded and given a name similar to registry’s name.

Secret becomes an authentication token when pulling images
* `kubectl patch serviceaccount default -p '{"imagePullSecrets": [{"name": "registry-kats-hello-world"}]}'`

Deploy app
* `kubectl create deployment my-python-app --image=registry.digitalocean.com/kats-hello-world/my-python-app`
* `kubectl get rs`
* `kubectl get pods`
* `kubectl scale deployment/my-python-app --replicas=20`
* `kubectl get pod -o=custom-columns=NODE:.spec.nodeName,NAME:.metadata.name --all-namespaces | grep my-python-app`

load balancing
* `kubectl expose deployment my-python-app --type=LoadBalancer --port=80 --target-port=80`
* `doctl compute load-balancer list --format Name,Created,IP,Status`




---
