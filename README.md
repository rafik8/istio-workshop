# Istio Service Mesh Workshop

[![Netlify Status](https://api.netlify.com/api/v1/badges/8833e1ba-0ec3-490c-8216-2a7e470bfba4/deploy-status)](https://app.netlify.com/sites/condescending-varahamihira-878695/deploys)

### Setup:

#### Install Hugo:
On a mac:

```shell
brew install hugo
```

On Linux:
  - Download from the releases page: https://github.com/gohugoio/hugo/releases/tag/v0.56.3
  - Extract and save the executable to `/usr/local/bin`

#### Clone this repo:
From wherever you checkout repos(or your fork):
```shell
git clone https://github.com/rafik8/istio-workshop.git
```

#### Clone the theme submodule:
```shell
cd istio-workshop
```

```shell
git submodule init
git submodule update
```

#### Run Hugo locally:
```shell
hugo server
```

#### View Hugo locally:
Visit http://localhost:1313/ to see the site.

#### Making Edits:
As you save edits to a page, the site will live-reload to show your changes.

#### Auto Deploy:
Any commits to master will auto build and deploy in a couple of minutes. You can see the currently
deployed hash at the bottom of the menu panel.

Any commits to a branch will auto build and deploy in a couple of minutes to a custom route named with the branch name. You can see the currently
deployed hash at the bottom of the menu panel.
