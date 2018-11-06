Here's how to update SSL using letsencrypt.

If you use manual mode, you'll need to rerun everything.
`certbot renew` won't work.

First, run `certbot certonly --manual`.

The script will give you a challenge file you need to serve via your Node server.

The remaining steps:
* Uploading the file to your repo.
* Verifying locally that the file is being served correctly.
* Deploying the server to GCP. (see [GCP cheat sheet](compute-engine.md))
* Continuing the certbot script to get the final `pem` files.
* Serving the `pem` files in your app.
* Re-deploying the server again.

## Verify SSH certificate

On a live domain:
```
echo | openssl s_client -connect [DOMAIN]:443 2>/dev/null | openssl x509 -noout -dates
```

For a local cert:
```
sudo openssl x509 -noout -dates -in /etc/letsencrypt/live/[DOMAIN]/cert.pem
```

Make sure the live domain is serving the correct cert.
