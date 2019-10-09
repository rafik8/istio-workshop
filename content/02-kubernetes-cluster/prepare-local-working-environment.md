---
title: "Prepare the local working environment"
chapter: true
weight: 1
---
# Prepare the local working environment
<!-- **Duration**: 5:00 -->

## Google Cloud account
We will be creating a cluster on Google’s Kubernetes Engine (GKE). We will provide **Google Cloud** free lab accounts to support this workshop during _SRECon EMEA 2019_.

{{% notice note %}}
To avoid conflicts with your personal account, please open a new incognito window for the rest of this lab.
![Incognito window](/images/incognito-window.png?width=10pc)
{{% /notice%}}


1. Sign in to the Google Cloud Platform Console: https://console.cloud.google.com/ with the provided credentials. In _Welcome to your new account dialog_, click Accept.

![Incognito window](/images/google-consent.png?width=30pc)

1. Then you have to approve Google Cloud Terms of Service, check **Terms of Service**, choose your country of residend and click **Agree and Continue**:

![Google Cloud Terms of Service](/images/googlecloud-terms.png?width=30pc)

1. If you see a top bar with **Activate Free Trial** - DO NOT ACTIVATE THE FREE TRIAL. Click **Dismiss** since we will be using a pre-provisioned lab accounts. If you are doing this on your own account (outside of SRECon workshop), then you may want the free trial.

![Google Cloud Trial Account](/images/googlecloud-trialaccount.png?width=50pc)

1. Click Select a project and select the project already created for the workshop:

![Google Cloud Select Project](/images/googlecloud-selectproject.png?width=50pc)

You should see the dashboard of the selected project:

![Google Cloud Select Project](/images/googlecloud-projectdashboard.png?width=50pc)

{{% notice info %}}
If you are running the workshop of your own, you have to create a project.
{{% /notice%}}


<!-- ## Google Cloud Shell
We could do most of the work from the [Google Cloud Shell](https://cloud.google.com/developer-shell/#how_do_i_get_started), a command line environment running in the Cloud. This Debian-based virtual machine is loaded with all the development tools you’ll need (docker, gcloud, kubectl and others) and offers a persistent 5GB home directory. Open the Google Cloud Shell by clicking on the icon on the top right of the screen:
![Google Cloud Shell](/images/googlecloud-shell.png?width=50pc)

1. When prompted, click Start Cloud Shell:

![Google Cloud Start Shell](/images/googlecloud-startshell.png?width=50pc)

Wait a couple of minutes when the shell machine being provisioned, you should see the shell prompt at the bottom of the window:
![Google Cloud Start Shell](/images/googlecloud-shellwindow.png?width=50pc)

{{% notice info %}}
When you run gcloud on your own machine, the config settings will be persisted across sessions.  But in Cloud Shell, you will need to set this for every new session / reconnection.
{{% /notice%}}

1. enable **Boost Mode** for Cloud Shell:

![Google Cloud Start Shell](/images/googlecloud-shellboostmode.png?width=30pc)

{{% notice tip %}}
Enable **Boost Mode** upgrades the shell instance from **g1-small** to **n1-standard-1**  which double the compute resources of the machine for the next 24 hours.
{{% /notice%}} -->

## Install gcloud CLI:

1. Install the gcloud CLI by following the instruction on : https://cloud.google.com/sdk/docs/downloads-interactive.

1. Configure your project with gcloud init.

```
gcloud version
```

```
Google Cloud SDK 264.0.0
beta 2019.05.17
bq 2.0.47
core 2019.09.22
gsutil 4.42
```


1. Configure your profile on `gcloud` account using :

```
gcloud init
```

1. Choose the option `[2] Create a new configuration` then choose a configuration name (for example: `istioworkshop-srecon19`)

1. Then choose option `[2] Log in with a new account`, Enter the email address of the Google Account provided for the training. You browser will be opened so you could grant access to gcloud cli.

![GCloud authorize](/images/gcloud-authorize.png?width=30pc)

1. Then pick up the project you have created:
```
Pick cloud project to use:
 [1] srecon19-workshop-250603
 ```


1. Finally, run the command `gcloud auth list` to check the the right account is used:

```shell
 gcloud auth list
           Credentialed Accounts
ACTIVE  ACCOUNT
        rafik.harabi@innovsquare.com
*       trainee001.srecon19@innovsquare.com

To set the active account, run:
    $ gcloud config set account 'ACCOUNT'
```


## Install kubectl CLI

1. Install the `kubectl` command-line tool:

```shell
gcloud components install kubectl
```

1. Check that kubectl is installed by using the command:

```
kubectl version
```
You will have the fellowing output:

```
Client Version: version.Info{Major:"1", Minor:"13+", GitVersion:"v1.13.11-dispatcher", GitCommit:"2e298c7e992f83f47af60cf4830b11c7370f6668", GitTreeState:"clean", BuildDate:"2019-09-
19T22:20:12Z", GoVersion:"go1.11.13", Compiler:"gc", Platform:"linux/amd64"}
The connection to the server localhost:8080 was refused - did you specify the right host or port?
```

The error regarding Kubernetes version which is normal as we haven't yet create a Kubernetes Cluster.
