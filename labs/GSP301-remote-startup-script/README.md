# GSP301 — Deploy a Compute Instance with a Remote Startup Script

This write-up documents the Google Cloud Skills Boost challenge lab **GSP301**. The lab provisions a Compute Engine VM and configures it to run an installation script stored in Cloud Storage at startup.

> This is a learning-lab implementation. The project ID, bucket name, VM external IP, and temporary lab credentials are intentionally not published.

## What this lab demonstrates

- Storing a startup script in Cloud Storage
- Creating a Debian 12 Compute Engine VM
- Configuring VM startup metadata with `startup-script-url`
- Applying a network tag to a VM
- Allowing HTTP traffic through a VPC firewall rule
- Verifying the web server over HTTP

## Environment used

| Resource | Configuration |
|---|---|
| Lab | GSP301 — Deploy a Compute Instance with a Remote Startup Script |
| VM name | `apache-vm` |
| Zone used in the lab session | `us-west1-b` |
| Machine type | `e2-medium` |
| Operating system | Debian 12 |
| Startup script object | `gs://<BUCKET_NAME>/install-web.sh` |
| Network tag | `http-server` |
| Firewall rule | `allow-http` — ingress TCP port 80 |

The zone above reflects the completed lab session. Use the zone supplied by your own Skills Boost lab if it differs.

## Workflow

### 1. Copy the startup script to Cloud Storage

The lab-provided script is available at `gs://spls/gsp301/install-web.sh`. Copy it into the bucket created for the lab:

```bash
export PROJECT_ID="$(gcloud config get-value project)"
export BUCKET_NAME="$PROJECT_ID"

gcloud storage cp gs://spls/gsp301/install-web.sh \
  "gs://$BUCKET_NAME/install-web.sh"
```

### 2. Create the VM with the remote startup script

```bash
export ZONE=us-west1-b

gcloud compute instances create apache-vm \
  --zone="$ZONE" \
  --machine-type=e2-medium \
  --image-family=debian-12 \
  --image-project=debian-cloud \
  --metadata=startup-script-url="gs://$BUCKET_NAME/install-web.sh" \
  --tags=http-server
```

The `startup-script-url` metadata key instructs the VM to retrieve and execute the script during startup.

### 3. Allow inbound HTTP traffic

```bash
gcloud compute firewall-rules create allow-http \
  --allow=tcp:80 \
  --target-tags=http-server \
  --description="Allow HTTP traffic to Apache web server"
```

### 4. Verify the web server

Get the VM's current external IP:

```bash
gcloud compute instances describe apache-vm \
  --zone="$ZONE" \
  --format="get(networkInterfaces[0].accessConfigs[0].natIP)"
```

Test the service, replacing `EXTERNAL_IP` with the returned address:

```bash
curl -I --max-time 20 http://EXTERNAL_IP
```

**Observed result:** `HTTP/1.1 200 OK`, served by Apache on Debian.

## Verification

- [x] VM `apache-vm` created and reached `RUNNING` state.
- [x] Remote startup script URL configured in instance metadata.
- [x] Firewall rule `allow-http` created for TCP port 80 and network tag `http-server`.
- [x] HTTP request returned `200 OK`.

## Skills demonstrated

Compute Engine, Cloud Storage, VM startup scripts, instance metadata, VPC firewall rules, Linux VM administration, and HTTP service validation.

## Security and cleanup

- Do not commit service-account keys, passwords, access tokens, or lab credentials.
- Do not publish temporary project IDs, external IP addresses, or bucket names.
- Remove lab resources only after confirming the Skills Boost session has ended and cleanup is appropriate.
