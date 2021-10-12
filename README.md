# Base Image for Kubernetes Administration

## Build Image

`docker-compose -f dist/alpine/docker-compose.yml --env-file \
    config/example.env build --no-cache`

## Launch locally


`docker-compose -f dist/alpine/docker-compose.yml --env-file config/example.env up --build`


## Notes

https://hub.docker.com/r/vathes/k8s-admin
