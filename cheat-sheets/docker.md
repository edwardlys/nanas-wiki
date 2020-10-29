---
title: Docker / Compose 
description: 
published: true
date: 2020-09-25T07:44:57.968Z
tags: 
editor: undefined
dateCreated: 2020-07-10T07:52:04.680Z
---

## Check Usage

Check CPU and memory usage for each container.

```bash
docker stats
```

## Enter Container

Enter a container interactively. Replace `bash` with `sh` depending on availability.

```bash
docker exec -ti <ContainerName> bash
```

```bash
docker-compose exec <ServiceName> bash
```

## Run Command

Run a command inside the docker container from host.

```bash
docker exec <ContainerName> <Command>
```

```bash
docker-compose exec <ServiceName> <Command>
```

## Inspect Container

Inspect the container details.

```bash
docker inspect <containerID> --format='{{range .NetworkSettings.Networks}}{{.Gateway}}{{end}}'
```

Use `range` to iterate through array items and `{{end}}` to end the query.

## Kill Individiual Container

```bash
docker kill <containerName>
```

```bash
docker-compose kill <ServiceName>
```

## Build Image

```bash
docker build
```

Options
- `--no-cache` Do not use cache
- `--tag <name>` Tag the image with a name
- `--file` Target a non standard named dockerfile

## Start A Container

```bash
cd <project-folder>
docker-compose up -d
```

```bash
docker run <image-name> --detach
```

- `--name` Name the container
- `--publish <host-port>:<container-port>` Publish port

## Start all stopped container

```bash
docker start $(docker ps -aq)
```