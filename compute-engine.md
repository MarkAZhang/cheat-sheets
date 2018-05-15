# Google Compute Engine

### Create instance with container
See [GCP docs](https://cloud.google.com/compute/docs/containers/deploying-containers)

```
# Install beta functions (first-time only).
gcloud components install beta

# Tag with http-server. See Firewall Rules below.
gcloud beta compute instances create-with-container my-container \
     --container-image gcr.io/${PROJECT_ID}/my-image:latest \
     --zone us-west1-b
     --machine-type f1-micro
     --tags http-server
```

### Update container on existing instance

NOTE: This will stop and restart the instance, causing it to change external IPs.

```
gcloud beta compute instances update-container my-container \
     --container-image gcr.io/${PROJECT_ID}/my-image:latest
```

### Configure Firewall Rule for HTTP
This will allow HTTP connections to all machines tagged with `http-server`.
See [GCP docs](https://cloud.google.com/compute/docs/containers/configuring-options-to-run-containers#publishing_container_ports).
```
gcloud compute firewall-rules create allow-http \
    --allow tcp:80 --target-tags http-server
```
