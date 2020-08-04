# gpu-lab

GPU enabled JupyterLab containers.

## Extended Container

- `txn2/gpu-lab-ex:v1.0.0`

```bash
docker build -t gpu-lab-ex:latest ./extended/
docker tag gpu-lab-ex:latest txn2/gpu-lab-ex:v1.0.0
```

## Base Container

- `txn2/gpu-lab-base:v1.0.0`

Re-building base image:
```bash
# clone the iot-salzburg/gpu-jupyter project
git clone https://github.com/iot-salzburg/gpu-jupyter.git

# generate gpu enabled base container
./gpu-jupyter/generate-Dockerfile.sh --no-useful-packages

# rebuild base container
docker build -t txn2/gpu-lab-base:latest ./gpu-jupyter/.build/

# tag container
docker tag txn2/gpu-lab-base:latest txn2/gpu-lab-base:v1.0.0

# test base lab container
docker run --rm -p 8888:8888 \
    --user root \
    -e GRANT_SUDO="yes" \
    -e JUPYTER_ENABLE_LAB="yes" \
    txn2/gpu-lab-base:v1.0.0
```