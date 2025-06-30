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
export AWS_PROFILE=<profile_name> && aws sso login --profile <profile_name>  # Replace <profile_name> with a profile from ~/.aws/config
```

Add the export statements to the bottom of your ~/.zshrc if you'd like these to be initialized in any new terminal session

## How do I run BriteCore?

> [!WARNING]
> Make sure that your Podman setup has enough resources allocated (≥ 8GB memory) to it in order to run a BriteCore site. The site is available once the `web` pod (seen using `bc-local status`) is `Running` all containers.

This will launch a local britecore site from scratch. 
```sh
bc-local bootstrap client=rowanmutual celery=false pytest=false #change args as needed
bc-local status
bc-local open web
```

## How can I use this to develop BriteCore?

```sh
bc-local refresh
```

Make updates to the BriteCore code in the directory specified at `$BRITECORE_CODE_DIR` then run `bc-local refresh`

## How can I use local monitoring tools?

```sh
bc-local monitoring
```

## How can I upgrade my bc-local version?

```sh
brew update && brew upgrade bc-local
```

## How can I get and use the beta version (less stable)
```sh
brew install bc-local-beta
bc-local-beta version
```
bc-local-beta is where we will push the latest features and changes that have been merged to the main source code branch.

## Troubleshooting

### ERROR: image: "bc-local/web" not present locally
If you're using podman and encountering a problem with `kind load docker-image...` when using `podman` where the error is something like `ERROR: image: "bc-local/web" not present locally` and you can see the image when using `podman images`, then the issue could be that your docker shim is not working properly and you can fix by running 

```sh
sudo ln -sf "$(command -v podman)" /usr/local/bin/docker
```

### ERROR: dropping 1005 traces to Datadog Agent ... ([Errno -2] Name or service not known) 
If you're seeing many error logs related to the DataDog Agent, it's typically because it's unable to find the bc-local monitoring stack. You can resolve this by launching the monitoring stack with `bc-local monitoring`

### ERROR: ECONNREFUSED 127.0.0.1:30678 (when connecting to debugger)
<img src="./docs/images/err_ECONNREFUSED_port_30678.png" alt="Screenshot of the VS Code error" width="300">

If you see this error when attempting to connect to the debugger port, this may be because the bc-local kind cluster is not exposing the port. You can test this by executing `lsof -i TCP |grep 30678` which should provide and output like `gvproxy ... TCP *:30678 (LISTEN)`. 

If you get no output then run the following to fix:
```
bc-local clean
bc-local bootstrap
```