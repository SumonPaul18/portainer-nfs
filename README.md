# Run Portainer and Data Store in NFS Server
####
    mkdir portainer & cd portainer
    nano docker-compose.yml
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
Now, Run Docker Compose UP Command
####
    docker compose up -d
####
