# Google Compute Engine

### Set Up

Create Project
```
gcloud create projects my-project

# List projects.
gcloud projects list

# You want project id, not project name.
gcloud config set project my-project

# Confirm project changed.
gcloud config get-value project
```

Build and Push Docker Container
```
export PROJECT_ID="$(gcloud config get-value project -q)"
echo $PROJECT_ID

docker build -t gcr.io/${PROJECT_ID}/my-app:latest .
gcloud docker -- push gcr.io/${PROJECT_ID}/my-app:latest
```

### Create instance with container
See [GCP docs](https://cloud.google.com/sdk/gcloud/reference/beta/compute/instances/create-with-container)

This is a convenience function. You can also create the instance first (with a Container-Optimized OS image), which comes with Docker installed, and then SSH into the instance and run the Docker image.

```
# Install beta functions (first-time only).
gcloud components install beta

# Tag with http-server. See Firewall Rules below.
gcloud beta compute instances create-with-container my-container \
     --container-image gcr.io/${PROJECT_ID}/my-image:latest \
     --zone us-west1-b \
     --machine-type f1-micro \
     --tags http-server
```

### Configure Firewall Rule for HTTP
This will allow HTTP connections to all machines tagged with `http-server`.
See [GCP docs](https://cloud.google.com/compute/docs/containers/configuring-options-to-run-containers#publishing_container_ports).
```
gcloud compute firewall-rules create allow-http \
    --allow tcp:80 --target-tags http-server
```

As stated [here](https://cloud.google.com/compute/docs/containers/configuring-options-to-run-containers#publishing_container_ports), the ports of the server will mirror that of the container. Since HTTP is on port 80, your container MUST listen on port 80.

### Update container on existing instance (no SSH)

NOTE: This will stop and restart the instance.
If you want to update the container without restarting the instance, you can SSH into the instance. However, you'll need to authenticate the Docker engine inside the instance in order to pull from Google Container Registry.

```
# my-container is the name of your instance.
gcloud compute instances list

gcloud beta compute instances update-container my-container \
     --container-image gcr.io/${PROJECT_ID}/my-app:latest
```

### SSH into instance

```
gcloud compute ssh my-instance-name
```

### Enable logging in Docker containers.

See [GCP Docs](https://cloud.google.com/community/tutorials/docker-gcplogs-driver)


```
# Set Google Cloud logging driver as default Docker logging driver.
echo '{"log-driver":"gcplogs"}' | sudo tee /etc/docker/daemon.json

# Restart Docker service.
sudo systemctl restart docker
```

Note: You will need to do this every time the instance restarts.

### Filter for GET / in GCP Logging.

The advanced search syntax is:

```
resource.type="global"
logName="projects/test-project-disaster-prep/logs/gcplogs-docker-driver"
jsonPayload.data:"GET / HTTP/1.1"
```

###
