# Extended Environment


### Test
```bash
docker run --name=lab --rm -p 8888:8888 --user=root -e GRANT_SUDO="yes" -e JUPYTER_ENABLE_LAB="yes" gpu-lab-ex:latest

docker exec -it lab bash

```

http://localhost:8888/lab
password: `asdf`