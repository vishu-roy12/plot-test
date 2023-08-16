# plot-test

## Sample repo creation ##

Created a sample public repo to save all the files.


## Provisioning the Cluster ##

Provisioned the minikube cluster on local system with just a single node.

Command used
```
minikube start --kubernetes-version=v1.22.3 --memory=2g --bootstrapper=kubeadm --extra-config=kubelet.authentication-token-webhook=true --extra-config=kubelet.authorization-mode=Webhook --extra-config=scheduler.address=0.0.0.0 --extra-config=controller-manager.address=0.0.0.0
```
## Dockerfile creation ##

Created Dockerfile and placed it in the app directory since i will be using the same repo for manifest deployment too, hence stored the file inside the app folder.

Docker image is creation is done by github-actions workflow and its getting pushed to docker hub. Below is the path for workflow and docker hub repo

[docker-image-build](https://github.com/vishu-roy12/plot-test/blob/main/.github/workflows/cicd.yaml)

[docker-image-save](https://hub.docker.com/repository/docker/royvishu1224/plot-test/general)


## Kustomize architecture

Using kustommized architecture for manifests deployment. Created a base folder and dev (overlays) folder. Base folder is where common configuration lives and overlays where you can modify the configuration as per the environment. The deployment will be done by ArgoCD.
To manually run kustomize against the manifest you can use below commands.

For dry run or build the yaml
```
kustomize build dev/
```

To apply the manifest
```
kubectl apply -k dev/
```
or
```
kustomize build dev/ | kubectl apply -f -
```

## Automated Deployments ##
For automated deployments to the environment we can use ArgoCD with kustomize with either the auto sync policy where whenever there is the change in the repo, arogocd will sync the recent change to the environment or doing manual sync where you go and click the sync button to deploy the manifest.