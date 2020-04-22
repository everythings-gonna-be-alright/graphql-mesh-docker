# GraphQL Mesh Docker

<img src="img/mesh.png" width=50px> on <img src="img/docker.png" width=50px>

Simple docker image to run [GraphQL Mesh](https://github.com/Urigo/graphql-mesh) (Thank you [Urigo](https://github.com/Urigo) and Mesh community for developing nice tool!)

## TL;DR

```sh
# Run on Docker
docker-compose up -d
# or run on Kubernetes
kubectl apply -f k8s
```

## Images for GraphQL Mesh

- [`graphql-mesh` on Dockerhub](https://hub.docker.com/repository/docker/hiroyukiosaki/graphql-mesh)
  - tag: `v0.1.9` ([Dockerfile](./Dockerfile))
    - Includes minimum CLI and handler ([`@graphql-mesh/openapi`](https://graphql-mesh.com/docs/handlers/openapi))
  - tag: `v0.1.9-all` ([Dockerfile-all](./Dockerfile))
    - Includes [all handlers](https://graphql-mesh.com/docs/handlers/available-handlers/)

## What is included in this repo

Resources for docker image building
- [Docker image source = Dockerfile](./Dockerfile)
- [docker-compose file](./docker-compose.yaml)

Test configuration for GraphQL Mesh for `docker-compse up`
- [./work](./work)

## Build

We can utilize `docker-compose` command to build with Dockerfile

```sh
docker-compose build
```

## Run

You can choose step for your envionment (Docker or Kubernetes)

### 1. On Docker 

Simply run this command.

```sh
docker-compose up -d
# or to run vX.X.X-all
docker-compose -f docker-compose-all.yaml up -d
```

After running docker image, we can access to GraphQL Mesh service at `http://localhost:4000`

### 2. On Kubernetes

```sh
# If you want to change image, please edit k8s/pod.yaml
kubectl apply -f k8s
```

## Customize

> [GraphQL Mesh Basic Usage](https://graphql-mesh.com/docs/getting-started/basic-example/)

- Please follow the guidance of the official document above and create your `.meshrc.yaml`.
- Place `.meshrc.yaml` in the directory.

  #### On Docker
  - Edit `docker-compose.yaml` file in order to point your `.meshrc.yaml`

  ```yaml
      volumes:
        - ./.meshrc.yaml:/work/.meshrc.yaml # <- comment out and ponit your .meshrc.yaml
  ```
  - Run `docker-compose up -d`

  #### On Kubernetes
  - **Overwrite** new ConfigMap resource file with this command.

  ```sh
  kubectl create cm meshrc-cm --from-file .meshrc.yaml --dry-run -o yaml> k8s/meshrc-cm.yaml
  ```
  - Run `kubectl apply -f k8s`
