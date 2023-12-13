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

