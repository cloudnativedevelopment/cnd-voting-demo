# Voting App CND demo

This repository contains a CND demo for the well known [voting app](https://github.com/dockersamples/example-voting-app).

## Classic Kubernetes Development

Classic Kubernetes Development refers to the development mode where developers build docker images and update their pods to test their changes. This mode introduces developer friction due to the **docker build redeploy** cycle.

Let's make a demo of this development mode using the Voting App.
The demo works in both, Docker for Mac (with Kubernetes support) and [minikube](https://github.com/kubernetes/minikube).
For minikube, remember to configure your docker client by running:

```bash
eval $(minikube docker-env)
```

Clone this repo and move to its root folder. Build the `voting:demo` image by executing:

```
docker build -t voting:demo vote
```

and run the Votting App by executing:

```
kubectl apply -f manifests
```

If you are running in Docker for Mac, the Voting App is available on `locahost:port 31000`.
If you are running in minikube, the Voting App is available on port 31000 in the minikube ip (`minikube ip`).

Note that if you click on "Cats" or "Dogs" in the Voting App UI, a container id is shown at the bottom.

Now, let's change the python app to return a fix value for this container id.

Edit the file `vote/app.py` and change the line 37 to be `hostname="classic"`. Save your changes.

In order to test the changes we need to rebuild or docker image by executing:

```
docker build -t voting:demo vote
```

and apply the kubernetes manifests again:

```
kubectl apply -f manifests
```

This introduces some friction, but even worse, if you check the Voting App UI and make a new vote, the code changes are not reflected. This is because we are using the same docker image tag. In order to refresh our pod we could create a different image tag and modify our `manifests/vote-deployment.yaml` to use the new docker image tag, or we can force the pod recreation by deleting the running pod. Let's go with the second approach and execute:

```
kubectl get pods
```

there will be a pod whose name starts with `vote-` (`vote-5d7889d8c9-lvpss`). Remove this pod by executing:

```
kubectl delete pods/vote-5d7889d8c9-lvpss
```

Wait a few seconds for kubernetes to deploy the new pod. Then go to the Voting App UI, make a vote, and finally your code changes will be live.


**Conclusion**: Classic Kubernetes Development introduces friction by requiring you to build images and redeploy them to your cluster to test every change. This substantially decreases productivity.

## Cloud Native Development

Cloud Native Development reduces friction by eliminating the **docker build redeploy** cycle.

Go to the repo root folder and execute:

```
cnd up
```

this will create a remote container which is synchronized with your local code changes and hot reloads these changes without rebuilding containers. Once `cnd up` is finished, execute:

```
cnd exec sh
```

The new terminal is running in the remote container. Run the python app by executing:

```
python app.py 
```

and check the Voting App UI is working properly.

Edit the file `vote/app.py` and change the line 37 to be `hostname="CND"`. Save your changes.

Finally, go to the Voting App UI, make another vote, and cool! your code changes are live!

**Conclusion**: Cloud Native Development reduces friction, increase productivity and enables team collaboration.





