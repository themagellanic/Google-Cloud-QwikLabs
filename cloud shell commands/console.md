### To know your porject id 
gcloud info --format='value(config.project)'
### To save it in a variable name PROJECT_ID
export PROJECT_ID=$(gcloud info --format='value(config.project)')

### To verify containers has been created 
gcloud container clusters list

### To get cluster credentials
gcloud container clusters get-credentials CLUSTER_NAME --zone SET_YOUR_ZONE

### Get the external IP of the application:
export EXTERNAL_IP=$(kubectl get service APPLICATION_NAME | awk 'BEGIN { cnt=0; } { cnt+=1; if (cnt > 1) print $4; }')

### To install a web server like Nginx. In SSH terminal of that instance run the command:
sudo apt-get install nginx-light -y
