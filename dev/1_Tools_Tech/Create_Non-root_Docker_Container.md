
#docker #devops #development #container-security

Ref: https://vsupalov.com/docker-shared-permissions/

```dockerfile
FROM ubuntu

ARG USER_ID
ARG GROUP_ID

RUN addgroup --gid $GROUP_ID user
RUN adduser --disabled-password --gecos '' --uid $USER_ID --gid $GROUP_ID user
USER user
```

Docker Run command:

```bash

$ docker build -t myimage \
  --build-arg USER_ID=$(id -u) \
  --build-arg GROUP_ID=$(id -g) .
$ docker run -it --rm \
  --mount "type=bind,src=$(pwd)/shared,dst=/opt/shared" \
  --workdir /opt/shared \
  myimage bash

```