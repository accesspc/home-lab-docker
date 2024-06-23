# Home Lab: Docker

This repo contains docker-compose yaml files to be run on a single Docker instance, and in this case - on a NAS with Docker capabilities.

In this case, it is a Lockerstor AS6704T.

## Docker metrics

```bash
cat <<EOF > /etc/docker/daemon.json
{
  "metrics-addr": "127.0.0.1:9323"
}
```

## Portainer

To deploy docker-compose yaml files will be using Portainer (Business Edition - can get a free licence key for up to 3 nodes). To deploy latest version of Portainer on NAS, need to enable Terminal, SSH onto it and run the following:

```bash
sudo -i
mkdir -p /share/Docker/Portainer/data

docker run \
  --name Portainer \
  -it -d \
  -p 19800:8000 -p 19900:9000 -p 19943:9443 \
  -v "/var/run/docker.sock:/var/run/docker.sock" \
  -v "/etc/localtime:/etc/localtime:ro" \
  -v "/usr/builtin/etc/certificate:/certs:ro" \
  -v "/share/Docker/Portainer/data:/data:rw" \
  --restart "unless-stopped" \
  --network "bridge" \
  --entrypoint "/portainer" \
  "portainer/portainer-ee:2.20.3" \
  "--sslcert" "/certs/ssl.pem" "--sslkey" "/certs/ssl.pem"
```

## Docker run builder

```bash
docker inspect --format "$(curl -s https://gist.githubusercontent.com/efrecon/8ce9c75d518b6eb863f667442d7bc679/raw/run.tpl)" name_or_id_of_running_container
```
