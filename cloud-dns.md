# Cloud DNS

### Create a new static external IP address.
```
gcloud compute addresses create [ADDRESS_NAME] --addresses [IP_ADDRESS] --region [REGION]
```

### List all external addresses assigned to current project.
```
gcloud compute addresses list
```

### Inspect firewall rules
```
gcloud compute firewall-rules list
gcloud compute firewall-rules describe [RULE_NAME]
```

### Point a custom domain to an external IP address.

See [GCP docs](https://cloud.google.com/dns/quickstart#before-you-begin).

### Assign external IP address to existing instance.

```
gcloud compute instances describe
```

Find the name of the existing access config under `networkInterfaces.accessConfigs`.

```
gcloud compute instances delete-access-config [INSTANCE_NAME] \
    --access-config-name "[ACCESS_CONFIG_NAME]"
    
gcloud compute instances add-access-config [INSTANCE_NAME] \
    --access-config-name "[ACCESS_CONFIG_NAME]" \
    --address [IP_ADDRESS]
```
