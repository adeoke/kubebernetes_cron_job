# Kubenetes cronjob example:

Theres often a need to create cron jobs within a kubernetes pod. This is an example of how I did it using my local minikube cluster.

# Demo

![Minikube Dashboard](/images/minikube_dashboard.png)

# Prerequisites:

You will need the following tools installed on your machine:

* [Kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/)
* [Minikube](https://kubernetes.io/docs/tutorials/hello-minikube/)
* [Docker](https://www.docker.com/get-started)

# Process:

Firstly, start Minikube:

```console
$ minikube start
```

Confirm that its started with:

```console
$ minikube status
```

Go into the minikube docker environment:

```console
$ eval $(minikube docker-env)
```

build then run the dockerfile, if running from the current diretory you just need to do the following (note i tagged my image simplejob:

```console
$ docker build . -t simplejob

$ docker run -it simplejob
```

Then use kubectl to generate a template job and pipe the output to job.yaml (note that this is already done, but if you want to do yourself then feel free to do so):

```console
$ kubectl create cronjob simplejob --image=simplejob --schedule="*/5 * * * *" --dry-run=client -o=yaml > job.yaml
```

Apply the job:
```console
$ kubectl apply -f job.yaml 
```

Wait a few seconds and then confirm that the job is being applied with command:

```console
kubectl get cronjobs
```

If you require it you can verify that the job is running through the ui by typing in the following:

```console
minikube dashboard --url=true
```

Then copying and pasting the output into your browser to view the cron jobs applied.

Jobe Done!
