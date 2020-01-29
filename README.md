# Traefik Certificate Extractor

Tool to extract Let's Encrypt certificates from Traefik's ACME storage file.

## Installation
```
git clone https://github.com/DanielHuisman/traefik-certificate-extractor
cd traefik-certificate-extractor
```

## Usage
```
python3 extractor.py [directory] [profile]
```
Default input directory is `./data`. The output directories are `./certs` and `./certs_flat`. Default input profile is `default`. The certificate extractor will extract certificates from any JSON file in the input directory (e.g. `acme.json`), so make sure this is the same as Traefik's ACME directory.

## Docker
There is a Docker image available for this tool: [guykhmel/traefik-certificate-extractor](https://hub.docker.com/r/guykhmel/traefik-certificate-extractor/).
Example run:
```
docker run --name extractor -d -v /srv/traefik/acme:/app/data -v /srv/extractor/certs:/app/certs -v /srv/extractor/certs_flat:/app/certs_flat guykhmel/traefik-certificate-extractor
```

### Docker-Compose
You can also use docker-compose
```
  traefik-certificate-extractor:
   image: guykhmel/traefik-certificate-extractor:latest
   container_name: traefik-certificate-extractor
   volumes:
    - /srv/traefik/acme:/app/data
    - /srv/certs:/app/certs:rw
    - /srv/certs/certs_flat:/app/certs_flat:rw
    - /var/run/docker.sock:/var/run/docker.sock
   restart: always
```

## Output
```
certs/
    example.com/
        cert.pem
        chain.pem
        fullchain.pem
        privkey.pem
    sub.example.nl/
        cert.pem
        chain.pem
        fullchain.pem
        privkey.pem
certs_flat/
    example.com.crt
    example.com.key
    example.com.chain.pem
    sub.example.nl.crt
    sub.example.nl.key
    sub.example.nl.chain.pem
```
