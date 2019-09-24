---
title: "Configure GCP account"
chapter: true
weight: 2
---
# Configure GCP account


1. Set the default project (replace **PROJECT_ID** with the project during the event workshop or your own project):

```
export PROJECT_ID=srecon19-workshop-250603
gcloud config set project $PROJECT_ID
```

2. Set the default compute region and zone:

```
export REGION_ID=europe-west1
gcloud config set compute/region $REGION_ID
```
```
export ZONE_ID=europe-west1-b
gcloud config set compute/zone $ZONE_ID
```

3. Check the configuration:
```
gcloud config list
```

```
[compute]
region = europe-west1
zone = europe-west1-b
[core]
account = trainee001.srecon19@innovsquare.com
disable_usage_reporting = True
project = srecon19-workshop-250603
```
