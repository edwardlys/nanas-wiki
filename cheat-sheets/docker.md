---
title: Docker / Compose 
description: 
published: true
date: 2020-07-10T07:52:04.680Z
tags: 
editor: markdown
---

## Check Usage

Check CPU and memory usage for each container

```bash
docker stats
```

```sh
CONTAINER ID        NAME                CPU %               MEM USAGE / LIMIT     MEM %               NET I/O             BLOCK I/O           PIDS
726774c43333        mimosa_app_1        0.04%               31.68MiB / 985.5MiB   3.21%               1.33MB / 1.85MB     123MB / 32.8kB      19
7fa51a8b9877        wiki_wiki_1         0.02%               99.88MiB / 985.5MiB   10.13%              110MB / 83.8MB      339MB / 922kB       15
ab6670859a34        wiki_db_1           0.64%               10.11MiB / 985.5MiB   1.03%               12.5MB / 11.1MB     72.9MB / 642MB      11
```

## Enter Container

Enter a container interactively. Replace `bash` with `sh` depending on availability

```bash
docker exec -ti <ContainerID> bash
```

```bash
docker-compose exec <ServiceName> bash
```

## Run Command

Run a command inside the docker container from host

```bash
docker exec <ContainerID> <Command>
```

```bash
docker-compose exec <ServiceName> <Command>
```