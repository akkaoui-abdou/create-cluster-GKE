Google Kubernetes Engine: Create GKE Cluster and Deploy Sample Website!!
========================================================================
Part - 1:
=======
Create a sample website using docker container

             export PROJECT_ID = kubernetes-366320

             docker run -p 8080:80 nginx:latest

             docker cp index.html [container-id]:/usr/share/nginx/html/
             
Create a new image from a containe:

             docker commit [container-id] abdel/nginx-web:version1
             
Create a tag for the image:

             docker tag abdel/nginx-web:version1 us.gcr.io/${PROJECT_ID}/nginx-web:version1
             
Create image in the registry (us.gcr.io):

             docker push us.gcr.io/${PROJECT_ID}/nginx-web:version1

PART - 2
=======
Deploying container in GKE cluster

             gcloud config set project ${PROJECT_ID}
             gcloud config set compute/zone us-central1-a

Creating a GKE cluster

            gcloud services list --available
 
            gcloud services enable containerregistry.googleapis.com
 
            gcloud auth configure-docker

             gcloud container clusters create abdel-cluster --num-nodes=1
 
             gcloud container clusters get-credentials abdel-cluster
 
  This command configures kubectl to use the cluster you created.

Deploying an application to the cluster:

             kubectl create deployment web-server --image=us.gcr.io/${PROJECT_ID}/nginx-web:version1

Exposing the Deployment:

 ``kubectl expose deployment web-server --type LoadBalancer --port 80 --target-port 80``

Inspecting and viewing the application
 1. Inspect the running Pods by using
 
            kubectl get pods
 2. Inspect the hello-server Service by using 
 
             kubectl get service

Sources:

 • https://cloud.google.com/container-registry/docs/pushing-and-pulling?_ga=2.111514180.-173018589.1570384092&hl=en_US

 • https://docs.docker.com/engine/reference/commandline/run/

 • https://cloud.google.com/sdk/gcloud/reference/auth/configure-docker

 • https://cloud.google.com/kubernetes-engine/docs/deploy-app-cluster
