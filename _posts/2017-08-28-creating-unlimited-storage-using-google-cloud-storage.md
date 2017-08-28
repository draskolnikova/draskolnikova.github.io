---
layout: post
title: Creating unlimited storage using Google Cloud Storage
categories: [work]
tags: [linux, centos, gcsfuse, gcp] 
---

Couple months ago, I have simple task but hardly to deploy. Creating huge/big storage for rawset/dataset but using very effience budget. Sounds impossible? Yes, it is. Because, to deploy redundant storage server it requires "huge" on everything (money, infrastructure, etc). The requirements is so simple, user stored assets, save it and access it.

So, I decided to put it on the cloud. I choose Google Cloud Storage (GCS) instead of AWS S3. Why? I need [FUSE](https://en.wikipedia.org/wiki/Filesystem_in_Userspace), and only GCS have [official FUSE support](https://github.com/GoogleCloudPlatform/gcsfuse), called `gcsfuse`.

By default, GCS will not allow you to create any files and directory then sync them to the cloud. But, they gave many parameters to allow you do that (like local file system on your linux).

## First Init
First thing first, don't forget to [install](https://cloud.google.com/sdk/downloads) and [auth](https://cloud.google.com/sdk/gcloud/reference/auth/login) your self on Google by follow the instruction. If you're successfully authorized, you should get information like this:

```
[jenna@galea ~]$ gcloud config list
[compute]
region = asia-east1
zone = asia-east1-a
[core]
account = [removed]
disable_usage_reporting = False
project = machine-learning-[removed]]

Your active configuration is: [default]
```

## Mounting buckets
I assume you already have buckets, if don't [please follow the instruction(s)](https://cloud.google.com/storage/docs/creating-buckets). The buckets called `buckets1`.

Please select or create your `key` by [following this instructions](https://developers.google.com/identity/protocols/application-default-credentials#howtheywork). If you have create and download the key, the key should be like this.


```
{
  "type": "service_account",
  "project_id": "machine-learning-[removed]",
  "private_key_id": "f3b9d87f5837200[removed]",
  "private_key": "YOUR_PRIVATE_KEY_HERE",
  "client_email": "[removed]-compute@developer.gserviceaccount.com",
  "client_id": "113014412[removed]",
  "auth_uri": "https://accounts.google.com/o/oauth2/auth",
  "token_uri": "https://accounts.google.com/o/oauth2/token",
  "auth_provider_x509_cert_url": "https://www.googleapis.com/oauth2/v1/certs",
  "client_x509_cert_url": "https://www.googleapis.com/robot/v1/metadata/x509/[removed]-compute%40developer.gserviceaccount.com"
}
```

Then you can place it anywhere, don't forget to chmod `600` otherwise the mount progress won't work. Then on `/etc/fstab`, put your configuration like this.

```
buckets1 /mnt/buckets1   gcsfuse rw,allow_other,file_mode=0644,dir_mode=0755,implicit_dirs,key_file=/root/key-files.json
```

Trying mount it using `mount -a` command, if you got no error, you should access the directory. Try to put some files/directory on this directory, it will appears also on the cloud and vice versa.

Hope it helps! :)
