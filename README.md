NVIDIA Riva for ASR and TTS on OpenShift
====

# Requirements

* An access to a [supported](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/support-matrix.html#support-matrix) GPU, e.g. V100 or T4 on [Equinix Metal](https://console.equinix.com/).
* Requirements as described in [Red Hat OpenShift on Equinix Metal with GPU](https://github.com/empovit/openshift-on-equinix-with-gpu#readme).
* Access to [NGC Catalog](https://catalog.ngc.nvidia.com/)
* Helm 3.x

# Preparation

Populate required and optional variables as described in the [playbook's documentation](https://github.com/empovit/openshift-on-equinix-with-gpu#readme).

In addition, add variables for pulling NVIDIA images from NGC.

```yaml
ngc_api_key: <NGC API key>
ngc_email: <NGC email>
```

Download the Riva API server Helm chart as described in [the Helm chart documentation](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/riva/helm-charts/riva-api).

Download Riva clients as described in the [Quick Start Guide](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/quick-start-guide.html).


Download a playbook for installing an SNO cluster on Equinix by running


```sh
ansible-playbook init.yml
```

# Running

## Riva API Server

Run the playbook

```sh
ansible-playbook riva.yml -e "@path/to/vars.yml"
```

In order to destroy the setup, run

```sh
ansible-playbook openshift-playbook/sno-cleanup.yml -e "@path/to/vars.yml"
```


In order to delete just a Riva deployment, run

```sh
helm delete -n riva riva-api
```

## Riva Client

Download Riva examples (see the Quick Start Guide), and run

```sh
python riva_quickstart_v2.2.1/examples/transcribe_file_offline.py --server <cluster_node>:<node_port> --audio-file en-US_sample.wav
```

# Additional Resources

* [Tutorials](https://github.com/nvidia-riva/tutorials)
* [Pre-trained models on NGC](https://catalog.ngc.nvidia.com/models?query=label:%22Riva%22)
* [Recording Audio from the User](https://web.dev/media-recording-audio/)
* [Independent audio samples](http://www.voiceage.com/Audio-Samples-AMR-WB.html)