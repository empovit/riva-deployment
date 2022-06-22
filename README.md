NVIDIA Riva for ASR and TTS on OpenShift
====

# Requirements

* A [supported](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/support-matrix.html#support-matrix) GPU, e.g. V100 or T4.
* An OpenShift cluster with GPU support
* Helm 3.x
* Access to [NGC Catalog](https://catalog.ngc.nvidia.com/)

# Required Changes

These are either required custom values such as passwords, adaptations of the Helm chart for OpenShift, or the implementation of a particular speech services use-case.

* Set `ngcCredentials.password`, `ngcCredentials.email`, and `modelRepoGenerator.modelDeployKey` as described in [Riva Speech Skills Helm chart documentation](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/riva/helm-charts/riva-api).

* Change the service type: modify _values.yaml_ or deploy with `--set service.type=NodePort`.

* `hostPath` storage volumes cannot be use "as is" with OpenShift, for now we will force the Helm chart to use `emptyDir`: `    --set modelDeployVolume="" --set artifactDeployVolume=""`


# Running

Deploy the Helm chart

with modified _values.yaml_:

`helm install --create-namespace -n riva riva-api riva-api`

by passing custom values in the command line:

```sh
NGC_API_KEY=<your NGC token>
NGC_EMAIL=<your NGC registration email>

helm install \
    --create-namespace -n riva \
    riva-api riva-api \
    --set ngcCredentials.password=`echo -n $NGC_API_KEY | base64 -w0` \
    --set ngcCredentials.email=$NGC_EMAIL \
    --set modelRepoGenerator.modelDeployKey=`echo -n tlt_encode | base64 -w0` \
    --set modelDeployVolume="" \
    --set artifactDeployVolume=""
```

Download Riva examples (see the Quick Start Guide), and run

`python riva_quickstart_v2.2.1/transcribe_file_rt.py --server <host>:50051 --audio-file en-US_sample.wav`


In order to delete the deployment, run `helm delete -n riva riva-api`


# Resources

* [Quick Start Guide](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/quick-start-guide.html)
* [Tutorials](https://github.com/nvidia-riva/tutorials)
* [Pre-trained models on NGC](https://catalog.ngc.nvidia.com/models?query=label:%22Riva%22)
* [Helm chart on NGC](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/riva/helm-charts/riva-api)
* [Recording Audio from the User](https://web.dev/media-recording-audio/)