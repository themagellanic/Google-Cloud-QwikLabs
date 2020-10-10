Task 1: Create a project jumphost instance
We will use this instance to perform maintenance for the project.

Make sure you:

1.name the instance nucleus-jumphost
2.use the machine type of f1-micro
3.use the default image type (Debian Linux)

**(gcloud_id) $ gcloud config set compute/zone us-east1-b**
**(gcloud_id) $ gcloud config set compute/zone us-east1**
**(gcloud_id)$ gcloud compute instances create nucleus-jumhost --machine-type f1-micro --zone us-east1-b**


Task 2: Create a Kubernetes service cluster
To create a cluster, run the following command.
(Note: Cluster names must start with a letter, end with an alphanumeric, and cannot be longer than 40 characters.)

1.Create a cluster (in the us-east1-b zone) to host the service
**(gcloud_id)$ gcloud container clusters get-credentials nucleus-jumhost-cluster**

Now to deploy containerized app .Kubernetes Engine uses Kubernetes objects to create and manage your cluster's resources. 
2.Use the Docker container hello-app (`gcr.io/google-samples/hello-app:2.0`) as a place holder, 
the team will replace the container with their own work later.
**(gcloud_id)$ kubectl create deployment hello-server --image=gcr.io/google-samples/hello-app:1.0**

3.Expose the app on port 8080
**(gcloud_id)$ kubectl expose deployment hello-server --type=LoadBalancer --port 8080**

Check status using 
**(gcloud_id)$ kubectl get service**


Task 3: Setup an HTTP load balancer
We will serve the site via nginx web servers, but we want to ensure we have a fault tolerant environment, 
so please create an HTTP load balancer with a managed instance group of two nginx web servers. Use the following to configure the 
web servers, the team will replace this with their own configuration later.

**(gcloud_id)$ cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF**

1.Create an instance template
**(gcloud_id)$ gcloud compute instance-templates create nginx-template \
         --metadata-from-file startup-script=startup.sh**
2.Create a target pool
**(gcloud_id)$ gcloud compute target-pools create nginx-pool**
3.Create a managed instance group
**(gcloud_id)$ gcloud compute instance-groups managed create nginx-group \
         --base-instance-name nginx \
         --size 2 \
         --template nginx-template \
         --target-pool nginx-pool**
**(gcloud_id)$ gcloud compute instances list**
4.Create a firewall rule to allow traffic (80/tcp)
**(gcloud_id)$ gcloud compute firewall-rules create www-firewall --allow tcp:80**
5.Create a health check
**(gcloud_id)$ gcloud compute http-health-checks create http-basic-check**
**(gcloud_id)$ gcloud compute instance-groups managed \
       set-named-ports nginx-group \
       --named-ports http:80**
6.Create a backend service and attach the manged instance group
**(gcloud_id)$ gcloud compute backend-services create nginx-backend \
      --protocol HTTP --http-health-checks http-basic-check --global**
**(gcloud_id)$ gcloud compute backend-services add-backend nginx-backend \
    --instance-group nginx-group \
    --instance-group-zone us-central1-a \
    --global**

7.Create a URL map and target HTTP proxy to route requests to your URL map
**(gcloud_id)$ gcloud compute url-maps create web-map \
    --default-service nginx-backend**
**(gcloud_id)$ gcloud compute target-http-proxies create http-lb-proxy \
--url-map web-map**
8.Create a forwarding rule
**(gcloud_id)$ gcloud compute forwarding-rules create http-content-rule \
--global \
--target-http-proxy http-lb-proxy \
--ports 80**

**(gcloud_id)$ gcloud compute forwarding-rules list**
