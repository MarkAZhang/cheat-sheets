# Cloud DNS

### Create a new static external IP address.
```
gcloud compute addresses create [ADDRESS_NAME] --addresses [IP_ADDRESS] --region [REGION]
```
Where IP_ADDRESS is an existing IP address that is assigned to an instance.

### List all external addresses assigned to current project.
```
gcloud compute addresses list
```

### Inspect firewall rules
```
gcloud compute firewall-rules list
gcloud compute firewall-rules describe [RULE_NAME]
```

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

### Point a custom domain to an external IP address.

1) [Enable the DNS API](https://console.cloud.google.com/flows/enableapi?apiid=dns)
2) [Create a new DNS Zone](https://console.cloud.google.com/net-services/dns/zones/~new)
3) Add an "A" record to the DNS zone to point to your external IP address.
4) Run `gcloud dns managed-zones describe ${DNS_ZONE_NAME}` to get nameservers for your zone.
5) Update your nameservers.

See [GCP docs](https://cloud.google.com/dns/quickstart#before-you-begin).
