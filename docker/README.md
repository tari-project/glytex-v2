# `graxil` in Docker

## NVIDIA Image

### Requirements

- [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html)

### Building

```shell
docker build -t graxil-cuda:latest -f docker/Dockerfile.cuda .
```

The following 3 build arguments are exposed for convenience:

- `BASE_CUDA_DEV_CONTAINER`
- `BASE_CUDA_RUN_CONTAINER`
- `RUST_TARGET_TYPE` (`debug` or `release`)

### Running

Assuming you want all GPUs:

```shell
docker run --name cuda0 -it --rm --gpus=all graxil-cuda:latest --help
```

## ROCm Image

- [ROCm Docker Documentation](https://rocm.docs.amd.com/projects/install-on-linux/en/latest/how-to/docker.html)
- The image currently runs as `root`, couldn't really find any way around that
  without bolting the image to the container host's setup.
  - Updated information is welcome here.
- At the moment, `graxil` does not support `rocm-smi` to capture stats, so will
  do GPU Mining, but report as "no-GPU." You can monitor stats with `rocm-smi`
  manually to confirm usage.

Of note, please decide for yourself if `--security-opt seccomp=unconfined` is for you.

### Building

The following 3 build arguments are exposed for convenience:

- `BASE_ROCM_DEV_CONTAINER`
- `BASE_ROCM_RUN_CONTAINER`
- `RUST_TARGET_TYPE` (`debug` or `release`)

```shell
docker build -t graxil-rocm:latest -f docker/Dockerfile.rocm .
```

### Running

```shell
docker run --name rocm0 --device /dev/kfd --device /dev/dri graxil-rocm:latest --help
```
