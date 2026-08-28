# Docker

List subnets for running containers
```bash
docker network inspect $(docker network ls -q)|grep "IPv4Address"
```
Include proxy information
```bash
docker run -E http_proxy=$http_proxy -E https_proxy=$https_proxy -E no_proxy=$no_proxy
docker build --build-arg http_proxy=$http_proxy --build-arg https_proxy=$https_proxy -build-arg no_proxy=$no_proxy
```
 