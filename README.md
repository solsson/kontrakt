## Kontrakt

_Kontrakt_s (pronounced contract) define build success.
It is language agnostic,
runs a bit like [ko](https://github.com/google/go-containerregistry/tree/master/cmd/ko)
and is inspired by [kompose](https://kompose.io/).

## Subcommands

Note that initially each command is a separate executable.

Maybe going forward that way too, 
so it can be individual steps in a Build task,
like the [lifecycle](https://github.com/buildpack/lifecycle) commands in [the riff buildpack build template]https://github.com/projectriff/riff-buildpack-group/blob/master/riff-cnb-buildtemplate.yaml.

### `kontrakt apply -R -f .`

Finds folders that match the _*kontrakt*_ naming convention and runs them in separate [namespaces](#namespaces).

The MVP here, to avoid a dependence on [image digest](#looking-up-the-image-digest) for now,
is that the only manipulation `apply` does is replace
`${kontrakt.sourceReference}`
whith the value of the env
`SOURCE_REFERENCE`.

And also replace `${KO_DOCKER_REPO}` (which is a repo hostname + optional subpath) with the env of the same name.
Note that unlike [ko](https://github.com/google/go-containerregistry/tree/master/cmd/ko)
we're not necessarily compatible with `ko.local`. Your specific kontrakts may be though.

Thus you should build and push with that reference as image tag.

#### `kontrakt imagereplace -i`

Part of `apply`. Implements the MVP step above, in-place updating source files.

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

### Integration with Build Pipeline

Example split of responsibilities between Build Pipeline (BP) and Kontrakt (KT):

 1. We're in a BP [PipelineRun](https://github.com/knative/build-pipeline/blob/master/docs/Concepts.md#pipelinerun) now, in a container in Kubernetes
 1. BP clones the git source (a [PipelineResource](https://github.com/knative/build-pipeline/blob/master/docs/Concepts.md#pipelineresources)) to a "workspace"
 1. BP executes 1+ build [Task](https://github.com/knative/build-pipeline/blob/master/docs/Concepts.md#task)s given by the pipeline
 1. Every build task `output`s an [Image Resource](https://github.com/knative/build-pipeline/blob/master/docs/using.md#image-resource)
 1. A `kontrakt` Task follows in the pipeline


### How you build stuff is up to you

... but Kontrakt can build it for you.
The only build method we support right now is [Kaniko]().

## Looking up the image digest

Prerequisite: The cluster that Kontrakt runs in and the cluster that it executes kontrakts on must be able to access the same image URLs. 

We're postponing tag-to-digest lookup as Knative has a lot of work-in-progress on that.

We recommend you tag all builds with the current git ref.
See https://github.com/knative/build-pipeline/issues/216#issuecomment-443540440.

TODO there should be a kontrakt util for such tagging, as the ref isn't known prior to the run.

## Dogfooding

This repository contain
