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
