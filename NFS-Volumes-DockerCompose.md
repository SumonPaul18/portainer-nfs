
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

