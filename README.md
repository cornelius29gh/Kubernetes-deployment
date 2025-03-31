# Kubernetes-deployment
Contains docker files / link to images from dockerhub / readme text for overview of implementation

Implementation is the creation of a cluster with some webservers and loadbalancers for each type (apache and apache-tomcat).

6 pods Apache -> Loadbalancer1
6 pods Apache-Tomcat -> Loadbalncer2


I have used GKE (google Kubernetes Engine) for this implementation / kubectl via goggle cloud shell CLI and the Loadbalancer type service in yaml files

#Creation of the cluster with 2 nodes
gcloud container clusters create cornelius-webserver-cluster   --zone us-central1-a   --num-nodes 2  # This creates 2 worker nodes in the cluster

#Connecting to created cluster
gcloud container clusters get-credentials cornelius-webserver-cluster --zone us-central1-a --project stoked-harbor-455322-c5

#I have uploaded the kubernetes-deployments.yaml file into cloud shell and ran the following command to create the deployments+services
kubectl apply -f kubernetes-deployments.yaml

#deployments and services have been created as viewed in detail via command

rusu_cornel_ionut@cloudshell:~ (stoked-harbor-455322-c5)$ kubectl get all -o wide
NAME                                     READY   STATUS    RESTARTS   AGE    IP           NODE                                                  NOMINATED NODE   READINESS GATES
pod/apache-deployment-8665fd4f6c-58mws   1/1     Running   0          7m3s   10.96.0.15   gke-cornelius-webserver--default-pool-436571fc-fdsk   <none>           <none>
pod/apache-deployment-8665fd4f6c-5p5sf   1/1     Running   0          7m3s   10.96.0.13   gke-cornelius-webserver--default-pool-436571fc-fdsk   <none>           <none>
pod/apache-deployment-8665fd4f6c-gt5l8   1/1     Running   0          7m3s   10.96.1.5    gke-cornelius-webserver--default-pool-436571fc-k4c5   <none>           <none>
pod/apache-deployment-8665fd4f6c-m4q4v   1/1     Running   0          7m3s   10.96.0.14   gke-cornelius-webserver--default-pool-436571fc-fdsk   <none>           <none>
pod/apache-deployment-8665fd4f6c-q858c   1/1     Running   0          7m3s   10.96.1.6    gke-cornelius-webserver--default-pool-436571fc-k4c5   <none>           <none>
pod/apache-deployment-8665fd4f6c-rfm7w   1/1     Running   0          7m3s   10.96.1.7    gke-cornelius-webserver--default-pool-436571fc-k4c5   <none>           <none>
pod/tomcat-deployment-6b56cf58c7-2zb69   1/1     Running   0          7m2s   10.96.1.10   gke-cornelius-webserver--default-pool-436571fc-k4c5   <none>           <none>
pod/tomcat-deployment-6b56cf58c7-5tn4l   1/1     Running   0          7m2s   10.96.1.8    gke-cornelius-webserver--default-pool-436571fc-k4c5   <none>           <none>
pod/tomcat-deployment-6b56cf58c7-8czgx   1/1     Running   0          7m2s   10.96.0.17   gke-cornelius-webserver--default-pool-436571fc-fdsk   <none>           <none>
pod/tomcat-deployment-6b56cf58c7-dm2bx   1/1     Running   0          7m2s   10.96.1.9    gke-cornelius-webserver--default-pool-436571fc-k4c5   <none>           <none>
pod/tomcat-deployment-6b56cf58c7-l8lmd   1/1     Running   0          7m2s   10.96.0.16   gke-cornelius-webserver--default-pool-436571fc-fdsk   <none>           <none>
pod/tomcat-deployment-6b56cf58c7-v7fxw   1/1     Running   0          7m2s   10.96.0.18   gke-cornelius-webserver--default-pool-436571fc-fdsk   <none>           <none>

NAME                     TYPE           CLUSTER-IP       EXTERNAL-IP     PORT(S)          AGE    SELECTOR
service/apache-service   LoadBalancer   34.118.226.114   34.72.35.206    80:30773/TCP     7m4s   app=apache
service/kubernetes       ClusterIP      34.118.224.1     <none>          443/TCP          27m    <none>
service/tomcat-service   LoadBalancer   34.118.233.241   34.171.113.69   8080:32727/TCP   7m3s   app=tomcat

NAME                                READY   UP-TO-DATE   AVAILABLE   AGE    CONTAINERS   IMAGES                                SELECTOR
deployment.apps/apache-deployment   6/6     6            6           7m5s   apache       cornelius29/apache:version1.0         app=apache
deployment.apps/tomcat-deployment   6/6     6            6           7m4s   tomcat       cornelius29/tomcat-image:version1.0   app=tomcat

NAME                                           DESIRED   CURRENT   READY   AGE    CONTAINERS   IMAGES                                SELECTOR
replicaset.apps/apache-deployment-8665fd4f6c   6         6         6       7m4s   apache       cornelius29/apache:version1.0         app=apache,pod-template-hash=8665fd4f6c
replicaset.apps/tomcat-deployment-6b56cf58c7   6         6         6       7m3s   tomcat       cornelius29/tomcat-image:version1.0   app=tomcat,pod-template-hash=6b56cf58c7


kubectl logs/describe to view further information

Visit http://34.72.35.206 (Apache service) in your browser.
Visit http://34.171.113.69:8080 (Tomcat service) in your browser.

This works until the GKE cornelius-webserver-cluster is running

