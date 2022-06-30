NVIDIA Riva for ASR and TTS on OpenShift
====

# Requirements

For both physical and virtual GPUs you need:

* Access to a plan with on of the [supported](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/support-matrix.html#support-matrix) GPU models on [Equinix Metal](https://console.equinix.com/)
* Access to [NGC Catalog](https://catalog.ngc.nvidia.com/)
* NGC CLI tool [installed and configured](https://ngc.nvidia.com/setup/installers/cli)
* [Helm 3](https://helm.sh/) installed

### For physical GPU

* Requirements as described in [Red Hat OpenShift on Equinix Metal with GPU](https://github.com/empovit/openshift-on-equinix-with-gpu#readme).

### For vGPU

* Requirements as described in [Deploying an OpenShift Cluster with vGPU on VMware vSphere](https://github.com/empovit/openshift-on-vmware-with-vgpu#readme).

# Preparation

Populate required and optional variables as described in the [GPU playbook's documentation](https://github.com/empovit/openshift-on-equinix-with-gpu#readme). If needed, populate also [the additional variables fot **vGPU**](https://github.com/empovit/openshift-on-vmware-with-vgpu#readme).

In addition, add variables for pulling NVIDIA images from NGC.

```yaml
ngc_api_key: <NGC API key>
ngc_email: <NGC email>
```

Download Riva artifacts and a playbook for installing an SNO cluster on Equinix by running:

for _physical GPU_

```sh
ansible-playbook init-gpu.yml -e "@path/to/vars.yml"
```

for _vGPU_

```sh
ansible-playbook init-vgpu.yml -e "@path/to/vars.yml"
```

# Running

## Riva API Server

Run the playbook

_Physical GPU_

```sh
ansible-playbook riva-gpu.yml -e "@path/to/vars.yml"
```

_vGPU_

```sh
ansible-playbook riva-vgpu.yml -e "@path/to/vars.yml"
```

In order to destroy the setup, run &mdash; depending on the GPU type (physical/virtual) &mdash; either

```sh
ansible-playbook gpu-openshift-playbook/sno-cleanup.yml -e "@path/to/vars.yml"
```

or

```sh
ansible-playbook vgpu-openshift-playbook/destroy.yml -e "@path/to/vars.yml"
```

In order to delete just a Riva deployment, run

```sh
export KUBECONFIG=...
helm delete -n riva riva-api
```

## Riva Client

Download Riva examples (see the Quick Start Guide), and run

```sh
python riva_quickstart_v2.2.1/examples/transcribe_file_offline.py --server <cluster_node>:<node_port> --audio-file <audio_sample.wav>
```

e.g.

```sh
python riva_quickstart_v2.2.1/examples/transcribe_file_offline.py --server 147.28.142.251:32222 --audio-file Sports.wav
```

# Additional Resources

* [Riva Helm chart documentation](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/riva/helm-charts/riva-api)
* [Riva Quick Start Guide](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/quick-start-guide.html)
* [Riva ASR Overview](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/asr/asr-overview.html)
* [Riva Tutorials](https://github.com/nvidia-riva/tutorials)
* [Pre-trained Riva models on NGC](https://catalog.ngc.nvidia.com/models?query=label:%22Riva%22)
* [Recording Audio from the User](https://web.dev/media-recording-audio/)
* [Independent audio samples](http://www.voiceage.com/Audio-Samples-AMR-WB.html)