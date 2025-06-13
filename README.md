# bc-local
Homebrew Repo for bc-local cask. Launch and run BriteCore Locally 🛳️

## How do I install?

> **Note:** Since this is a homebrew cask there is no need to clone this repository. You just need to have [homebrew](https://brew.sh) installed!

```
brew tap intuitivewebsolutions/bc-local
brew install bc-local
```

## Pre-requisites for running bc-local

```sh
export BRITECORE_CODE_DIR=<path/to/britecore/code/dir>
mkdir -p ~/bc-local-data-load
export BRITECORE_DATA_LOAD_DIR=~/bc-local-data-load
export KIND_EXPERIMENTAL_PROVIDER=podman
aws sso login
```

## How do I run BriteCore?

> [!WARNING]
> Make sure that your Podman setup has enough resources allocated (≥ 8GB memory) to it in order to run a BriteCore site. The site is available once the `web` pod (seen using `bc-local status`) is `Running` all containers.

This will launch a local britecore site from scratch. 
```sh
bc-local bootstrap
bc-local status
bc-local open web
```

## How can I use this to develop BriteCore?

```sh
bc-local refresh
```

Make updates to the BriteCore code in the directory specified at `$BRITECORE_CODE_DIR` then run `bc-local refresh`

## How can I upgrade my version?

```sh
brew update
brew upgrade bc-local
```

## Troubleshooting

### ERROR: image: "bc-local/web" not present locally
If you're using podman and encountering a problem with `kind load docker-image...` when using `podman` where the error is something like `ERROR: image: "bc-local/web" not present locally` and you can see the image when using `podman images`, then the issue could be that your docker shim is not working properly and you can fix by running 

```sh
sudo ln -sf "$(command -v podman)" /usr/local/bin/docker
````