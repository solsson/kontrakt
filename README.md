## Kontrakt

_Kontrakt_s (pronounced contract) define build success.
It is language agnostic,
runs a bit like [ko](https://github.com/google/go-containerregistry/tree/master/cmd/ko)
and is inspired by [kompose](https://kompose.io/).

## Subcommands



### `kontrakt apply -R -f .`

Finds folders that match the _*kontrakt*_ naming convention and runs them in separate [namespaces](#namespaces).

### `kontrakt wait`

### `kontrakt assert`

### `kontrakt push`

### `kontrakt delete`

### `kontrakt 


## Yamls

Images to be built can be defined as in [ko](https://github.com/google/go-containerregistry/tree/master/cmd/ko),
with a source URI,
but also as in docker-compose with a relative path.

Unlike docker-compose we encourage one file per resource
because it makes it easier to take "inspiration" from other _kontrakt_s.

## Labels

Pods with a label `kontrakt: target` run images to be pushed upon a successful run.
These images must have a digest.

Pods with a label `kontrakt: test` are images that define test run success.

## Test namespace

Every kontrakt gets a temporary namespaces.

It gets a service account with full access to this namespace,
but no access to other namespaces or global resources.

## The relation to Knative Build Pipeline

TODO How do we run kontrakt as tasks

TODO would it be better if itests are also tasks?
  - but pipelines aren't dynamic that way

TODO how do we stay compatible with the "cluster" resource type in knative,
and also to in-cluster execution.

There are two types of modules, those that follow a template and those that don't.
If you follow a template it might very well executute your _kontrakt_s,
but the pipeline definition lives outside your repository.

With a non-standard project you might maintain the [Pipeleine]() yaml in the repository,
as some branches may affect the source and the pipeline together.

Regardless of which, responsibilities should be clear:

 * _Kontrakt_s

 ## Dogfooding

 This repository contain
