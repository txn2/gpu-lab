# gpu-lab

GPU enabled JupyterLab containers.

## Latest Extended Container

- `txn2/gpu-lab-ex:v2.0.1`

```bash
docker build -t txn2/gpu-lab-ex:latest ./extended/
docker tag txn2/gpu-lab-ex:latest txn2/gpu-lab-ex:v2.0.1
docker push txn2/gpu-lab-ex:latest
docker push txn2/gpu-lab-ex:v2.0.1

# test container
docker run --rm --name lab -p 8888:8888 --user root -e GRANT_SUDO="yes" -e JUPYTER_ENABLE_LAB="yes" txn2/gpu-lab-ex:v2.0.0

# root exec
docker exec -it lab bash

```

## Latest Base Container

- `txn2/gpu-lab-base:v2.0.0`

Re-building base container:
```bash
# rebuild base container
docker build -t txn2/gpu-lab-base:latest ./base/

# tag container
docker tag txn2/gpu-lab-base:latest txn2/gpu-lab-base:v2.0.1
docker push txn2/gpu-lab-base:latest
docker push txn2/gpu-lab-base:v2.0.1

# test base lab container
docker run --rm --name lab -p 8888:8888 --user root -e GRANT_SUDO="yes" -e JUPYTER_ENABLE_LAB="yes" txn2/gpu-lab-base:v2.0.1
```

## GPU Jupyter

Re-building GPU Jupyter Dockerfile:
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
docker run --rm --name lab -p 8888:8888 --user root -e GRANT_SUDO="yes" -e JUPYTER_ENABLE_LAB="yes" txn2/gpu-lab-base:v1.0.0

```