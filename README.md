# Google Cloud Compute – NGINX Lab

## Google Cloud Skills Boost

**Lab:** The Basics of Google Cloud Compute: Challenge Lab  
**Lab ID:** ARC120  
**Level:** Introductory

## Objective

Build the infrastructure for a web application using Google Cloud Compute Engine and Cloud Storage, then install and verify an NGINX web server.

## Lab Configuration

| Resource | Configuration |
|---|---|
| Region | `us-east4` |
| Zone | `us-east4-a` |
| Compute Engine instance | `my-instance` |
| Machine type | `e2-medium` |
| Boot disk | 10 GB balanced persistent disk |
| OS | Debian GNU/Linux 12 (bookworm) |
| Persistent disk | `mydisk` |
| Persistent disk size | 200 GB |
| HTTP traffic | Enabled |
| Cloud Storage bucket | `qwiklabs-gcp-02-60957f0a0a5d-bucket` |

## Tasks

### Task 1 – Create a Cloud Storage bucket

Created the required Cloud Storage bucket in the US multi-region:

```text
qwiklabs-gcp-02-60957f0a0a5d-bucket
```

### Task 2 – Create and attach a persistent disk

Created Compute Engine instance `my-instance` using an E2 `e2-medium` machine type with a 10 GB balanced persistent boot disk and Debian 12.

Created the 200 GB balanced persistent disk:

```text
mydisk
```

The persistent disk was attached to `my-instance`.

### Task 3 – Install NGINX

SSH into the Compute Engine VM and update the operating system:

```bash
sudo apt-get update
```

Install NGINX:

```bash
sudo apt-get install -y nginx
```

Verify the NGINX service:

```bash
sudo systemctl status nginx
```

Expected status:

```text
Active: active (running)
```

Test the web server locally:

```bash
curl http://localhost
```

The response should contain:

```text
Welcome to nginx!
```

## Verification

The external IP of the VM can be opened in a browser using:

```text
http://EXTERNAL_IP/
```

The default NGINX page should display **Welcome to nginx!**.

## Notes

This repository documents the infrastructure and commands used for the Google Cloud Skills Boost ARC120 challenge lab. Temporary lab credentials, SSH private keys, passwords, and other secrets must not be committed to this repository.

## Additional lab write-up

- [GSP301 — Deploy a Compute Instance with a Remote Startup Script](labs/GSP301-remote-startup-script/README.md)
