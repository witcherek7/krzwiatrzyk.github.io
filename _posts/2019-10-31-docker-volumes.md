




## Docker Volumes

We have several types of volume management. I will try to describe them as simple as I can! (And correctly, I hope)

### <b>Local container volume</b>

For local container volume we can specify inside the Dockerfile:
```
VOLUME volume1_name volume2_name
```

To understand: https://stackoverflow.com/questions/41935435/understanding-volume-instruction-in-dockerfile

or within run command:
```
docker container run -v /volume3_name IMAGE
```

We can access data from that volumes in:
```
docker volume ls
/var/lib/docker/volumes/VOLUME_ID/_data
```

### <b> Host mounted volume </b>

To mount volume from the host with path in the project directory:
```
docker container run -v $(pwd)/src:/src IMAGE
```

### <b> Shared volume </b>

Shared volume is more permanent as many containers can use it:
```
docker volume create --name volume_name
docker container run -v volume_name:/path/to/mount IMAGE
```

Mounting volume will copy files from container into the volume if specified path is not empty.
```
docker container run -v volume_name:/var IMAGE
docker container run -v volume_name:/volume_name_dir IMAGE
```
In the example /volume_name_dir will contain files from /var.

### <b> Local volume shared beetwen containers </b>

```
docker container run --name CONT1 -v DataVolume:/datavolume IMAGE
docker container run --name CONT2 --volumes-from CONT1 IMAGE
```

### <b> Volume permission </b>

Mounting volume with permissions:
```
docker container run --name CONT3 --volumes-from CONT1:ro IMAGE
docker container run --name CONT4 -v myvol:/path:rw
```

### RULE: Named volumes are not deleted on containers deletion, anonymous are deleted.
