NVIDIA Riva for ASR and TTS on OpenShift
====

# Requirements

* An access to a [supported](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/support-matrix.html#support-matrix) GPU, e.g. V100 or T4 on [Equinix Metal](https://console.equinix.com/).
* Requirements as described in (Red Hat OpenShift on Equinix Metal with GPU)[https://github.com/empovit/openshift-on-equinix-with-gpu#readme]
* Access to [NGC Catalog](https://catalog.ngc.nvidia.com/)
* Helm 3.x

# Preparation

Download the Riva API server Helm chart as described in [the Helm chart documentation](https://catalog.ngc.nvidia.com/orgs/nvidia/teams/riva/helm-charts/riva-api).

Download Riva clients as described in the [Quick Start Guide](https://docs.nvidia.com/deeplearning/riva/user-guide/docs/quick-start-guide.html).


Run `ansible-playbook init.yml`. This command will download a playbook for installing an SNO cluster on Equinix.

Populate required variables as described in the [playbook's documentation](https://github.com/empovit/openshift-on-equinix-with-gpu#readme).

In addition, add values for pulling NVIDIA images from NGC.

```yaml
ngc_api_key: <NGC API key>
ngc_email: <NGC email>
```

# Running

## Riva API Server

Running the playbook: `ansible-playbook riva.yml -e "@path/to/vars.yml`

In order to delete the deployment, run: `helm delete -n riva riva-api`

In order to destroy the entire setup, run: `ansible-playbook openshift-playbook/sno-cleanup.yml -e "@path/to/vars.yml"`

## Riva Client

Download Riva examples (see the Quick Start Guide), and run

`python riva_quickstart_v2.2.1/transcribe_file_rt.py --server <host>:50051 --audio-file en-US_sample.wav`

# Additional Resources

* [Tutorials](https://github.com/nvidia-riva/tutorials)
* [Pre-trained models on NGC](https://catalog.ngc.nvidia.com/models?query=label:%22Riva%22)
* [Recording Audio from the User](https://web.dev/media-recording-audio/)