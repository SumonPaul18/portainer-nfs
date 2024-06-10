
### Mounting NFS Volumes with Docker Compose
If you use Docker Compose to manage your containers, mount the NFS volume by defining it in the YML file.
####
Create the YML file.
####
    nano docker-compose.yml
####
Define the NFS volume in the volumes section.
####
    version: "2.2"
    services:
      portainer:
        image: portainer/portainer
        ports:
          - "9000:9000"
        volumes:
          - type: volume
          source: nfs-volume
          target: /nfs
          volume:
            nocopy: true
    volumes:
      nfs-volume:
        driver_opts:
          type: "nfs"
          o: "addr=192.168.0.96,nolock,soft,rw"
          device: ":/nfs-share/docker/porainer-data"
####
    docker compose up -d
####
### Create NFS Docker Volume
The details include:

- The volume type.
- The write mode.
- The IP or web address of the remote NFS server.
- The path to the shared directory on the server.
####
NFS Docker volume Syntax
####
    docker volume create --driver local \
      --opt type=nfs \
      --opt o=addr=[ip-address],rw \
      --opt device=:[path-to-directory] \
      [volume-name]
####
List the available Docker volumes.
####
    docker volume ls
####
Inspect the volume with the inspect subcommand.
####
    docker volume inspect [volume-name]

####
Use the docker run command to start the container. Specify the NFS volume and the mount point in the --mount section.
####
    docker run -d -it \
      --name [container-name] \
      --mount source=[volume-name],target=[mount-point]\
      [image-name]


####
    version: "2.2"
    services:
      portainer:
        image: portainer/portainer-ce:latest
        container_name: portainer
        restart: always
        ports:
          - "8000:8000"
          - "9443:9443"
        volumes:
          - /var/run/docker.sock:/var/run/docker.sock
          - nfsvolume-portainer:/data:rw
        tty: true

    volumes:
      nfsvolume-portainer:
        driver: local
        driver_opts:
          type: "nfs"
          o: "addr=192.168.0.96,rw,nfsvers=4"
          device: ":/nfs-share/docker/portainer-data"

####
    chown -R nobody:nogroup docker
    chmod -R 755 docker



