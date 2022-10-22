Google Kubernetes Engine: Create GKE Cluster and Deploy Sample Website!!
========================================================================
Part - 1:
=======
Create a sample website using docker container

``docker run -p 8080:80 nginx:latest``

``docker cp index.html [container-id]:/usr/share/nginx/html/``

``docker commit [container-id] cad/web:version1``

``docker tag cad/web:version1 us.gcr.io/youtube-demo-255723/cad-site:version1``

``docker push us.gcr.io/youtube-demo-255723/cad-site:version1``

PART - 2
=======
Deploying container in GKE cluster

``gcloud config set project youtube-demo-255723``
 ``gcloud config set compute/zone us-central1-a``

Creating a GKE cluster

 ``gcloud container clusters create gk-cluster --num-nodes=1``
 
 ``gcloud container clusters get-credentials gk-cluster``
 
  This command configures kubectl to use the cluster you created.

Deploying an application to the cluster:

 ``kubectl create deployment web-server --image=us.gcr.io/youtube-demo-255723/cad-site:version1``

Exposing the Deployment:

 ``kubectl expose deployment web-server --type LoadBalancer --port 80 --target-port 80``

Inspecting and viewing the application
 1. Inspect the running Pods by using
 
            ``kubectl get pods``
 2. Inspect the hello-server Service by using 
 
            `` kubectl get service``

Sources:

 • https://cloud.google.com/container-registry/docs/pushing-and-pulling?_ga=2.111514180.-173018589.1570384092&hl=en_US

 • https://docs.docker.com/engine/reference/commandline/run/

 • https://cloud.google.com/sdk/gcloud/reference/auth/configure-docker

 • https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster
