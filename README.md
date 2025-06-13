# bc-local
Homebrew Repo for bc-local cask. Launch and run BriteCore Locally

# Intuitivewebsolution Bc-local

## How do I install these formulae?

```
brew tap intuitivewebsolutions/bc-local
brew install bc-local
```

## Pre-requisites for running bc-local

```sh
export BRITECORE_CODE_DIR=<path/to/britecore/code/dir>
export BRITECORE_DATA_LOAD_DIR=~/bc-local-data-load
export KIND_EXPERIMENTAL_PROVIDER=podman
```

## How do I run BriteCore?

```sh
bc-local bootstrap
bc-local status
bc-local open web
```

This will launch a local britecore site from scratch. Make sure that your Podman setup has enough resources allocated (≥ 8GB memory) to it in order to run a BriteCore site. The site is available once the `web` pod (seen using `bc-local status`) is `Running` all containers.

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