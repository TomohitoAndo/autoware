# spconv

This role install the `cumm` and `spconv` libraries needed to perform sparse convolutions.
The [original implementation](https://github.com/traveller59/spconv) did not provide a shared library, which is pre-generated c++ code and pre-compiled libraries were prepared [separately](https://github.com/autowarefoundation/spconv_cpp).

## CUDA tag selection

The role selects the spconv_cpp release tag from `cuda_version` (same `-e cuda_version=…` passed to the cuda role):

| CUDA version | Release tag | arm64 deb suffix |
| ------------ | ----------- | ---------------- |
| 13.x         | `cu130-rev1` | none (plain `arm64`) |
| 12.x         | `cu128-rev1` | `-sbsa` or `-jetson` |

Override directly with `-e spconv_cuda_tag=cu128-rev1` if needed.

## Architecture Support

This role supports different architectures with platform-specific package variants:

- **amd64**: No suffix (e.g., `cumm_0.5.3_amd64.deb`)
- **arm64** (`cu128-rev1`):
  - Default: `-sbsa` suffix for server platforms (e.g., `cumm_0.5.3_arm64-sbsa.deb`)
  - Jetson: `-jetson` suffix when `spconv_is_jetson=true` (e.g., `cumm_0.5.3_arm64-jetson.deb`)
- **arm64** (`cu130-rev1`): plain `arm64` (e.g., `cumm_0.5.3_arm64.deb`)

## Manual Installation

For manual installation, please follow the instructions in [this](https://github.com/autowarefoundation/spconv_cpp) repository.

## Run the playbook

The following command will install a particular version of the packages using ansible.

### Standard installation (amd64 or arm64 server)

```bash
export SPCONV_CUMM_VERSION=0.5.3
export SPCONV_VERSION=2.3.8
ansible-playbook autoware.dev_env.install_dev_env --tags spconv -e spconv_cumm_version=${SPCONV_CUMM_VERSION} -e spconv_version=${SPCONV_VERSION} --ask-become-pass
```

### Installation for NVIDIA Jetson platforms

```bash
export SPCONV_CUMM_VERSION=0.5.3
export SPCONV_VERSION=2.3.8
ansible-playbook autoware.dev_env.install_dev_env --tags spconv -e spconv_cumm_version=${SPCONV_CUMM_VERSION} -e spconv_version=${SPCONV_VERSION} -e spconv_is_jetson=true --ask-become-pass
```
