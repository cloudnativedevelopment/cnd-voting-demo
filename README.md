# Voting App CND demo

Develop a simple distributed application the cloud native way.

## Getting started
`kube-deployment.yml` contains the specifiations of the Voting App's services.

First create the vote namespace
```
kubectl create namespace vote
```

Run the following command to create the deployments and services objects

```
$ kubectl create -f kube-deployment.yml --namespace vote
service/redis created
deployment.apps/redis created
service/db created
deployment.apps/db created
persistentvolumeclaim/postgres-pv-claim created
service/result created
deployment.apps/result created
service/vote created
deployment.apps/vote created
service/worker created
deployment.apps/worker created
```

Run the following command to get the ip and port of the vote service

```
$ kubectl get service vote --namespace=vote
NAME   TYPE           CLUSTER-IP      EXTERNAL-IP      PORT(S)          AGE
vote   LoadBalancer   10.15.249.191   35.204.101.246   5000:30250/TCP   4m41s
```

Open a browser and navigate to the vote service's UI (e.g. http://35.204.101.246:5000) to make sure that everything is in order.

Feel free to review the [Voting App official repo](https://github.com/dockersamples/example-voting-app) if you want more information on the app.

## Cloud Native Development

[Cloud Native Development](https://github.com/okteto/cnd) helps you go faster friction by eliminating the **docker build redeploy** cycle.

Run the following command to start your [cloud native environment](https://github.com/okteto/cnd#cloud-native-development-cnd)

```
cnd up
```

This will create a remote container which is synchronized with your local code changes and hot reloads these changes without rebuilding containers. 

Once `cnd up` is finished, open a browser and navigate to the vote service's UI. 

Edit the file `vote/app.py` and change line 8 from `cats` to `otters` and save. 

Go to the Voting App UI, make another vote. Your code changes are already live!!

Review [cnd's usage](https://github.com/okteto/cnd#usage) guide to see other commands available to help you speed you up your development.

Cancel the `cnd up` command by pressing `ctrl + c` and run the following command to go back to the production version of the service

```
cnd down
``` 

## Cleanup
Run the following commands to remove the resources created by the commands above 

```
$ kubectl delete -f kube-deployment.yml --namespace vote
service "redis" deleted
deployment.apps "redis" deleted
service "db" deleted
deployment.apps "db" deleted
persistentvolumeclaim "postgres-pv-claim" deleted
service "result" deleted
deployment.apps "result" deleted
service "vote" deleted
deployment.apps "vote" deleted
service "worker" deleted
deployment.apps "worker" deleted
```
