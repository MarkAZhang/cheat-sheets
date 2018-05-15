# Cloud DNS

### Create a new static external IP address.
```
gcloud compute addresses create my-address --addresses 35.100.100.100 --region us-west1
```

### List all external addresses assigned to current project.
```
gcloud compute addresses list
```

### Point a custom domain to an external IP address.

See [GCP docs](https://cloud.google.com/dns/quickstart#before-you-begin).
