[![Travis](https://img.shields.io/travis/oznu/docker-cloudflare-ddns.svg)](https://travis-ci.org/oznu/docker-cloudflare-ddns) [![Docker Pulls](https://img.shields.io/docker/pulls/oznu/cloudflare-ddns.svg)](https://hub.docker.com/r/oznu/cloudflare-ddns/)

# Docker CloudFlare DDNS

This small Alpine Linux based Docker image will allow you to use the free [CloudFlare DNS Service](https://www.cloudflare.com/dns/) as a Dynamic DNS Provider ([DDNS](https://en.wikipedia.org/wiki/Dynamic_DNS)).

## Image Variants

| Image Tag             | Architecture  | OS            | Size   |
| :-------------------- | :-------------| :------------ | :----  |
| latest                | x64           | Alpine Linux  | [![](https://images.microbadger.com/badges/image/oznu/cloudflare-ddns.svg)](https://microbadger.com/images/oznu/cloudflare-ddns) |
| armhf          | arm32v6       | Alpine Linux  | [![](https://images.microbadger.com/badges/image/oznu/cloudflare-ddns:armhf.svg)](https://microbadger.com/images/oznu/cloudflare-ddns:armhf) |
| aarch64         | arm64       | Alpine Linux  | [![](https://images.microbadger.com/badges/image/oznu/cloudflare-ddns:aarch64.svg)](https://microbadger.com/images/oznu/cloudflare-ddns:aarch64) |

## Usage

Quick Setup:

```shell
docker run \
  -e API_TOKEN=xxxxxxx \
  -e ZONE=example.com \
  -e ZONE_ID=xxxxxxx \
  -e SUBDOMAIN=subdomain \
  oznu/cloudflare-ddns
```

This image will also run on a Raspberry Pi or other ARM based boards that support Docker using the `armhf` or `aarch64` tags:

```shell
docker run \
  -e API_TOKEN=xxxxxxx \
  -e ZONE=example.com \
  -e ZONE_ID=xxxxxxx \
  -e SUBDOMAIN=subdomain \
  oznu/cloudflare-ddns:armhf
```

## Parameters

* `--restart=always` - ensure the container restarts automatically after host reboot.
* `-e API_TOKEN` - Your CloudFlare API Token. Get it here: https://dash.cloudflare.com/profile/api-tokens. **Required**
* `-e ZONE` - The DNS zone that DDNS updates should be applied to. **Required**
* `-e ZONE_ID` - The DNS zone ID that matches the provided ZONE. **Required**
* `-e SUBDOMAIN` - A subdomain of the `ZONE` to write DNS changes to. If this is not supplied the root zone will be used.
* `-e PROXIED` - Set to `true` to make traffic go through the CloudFlare CDN. Defaults to `false`.
* `-e RRTYPE=A` - Set to `AAAA` to use set IPv6 records instead of IPv4 records. Defaults to `A` for IPv4 records.
* `-e DELETE_ON_STOP` - Set to `true` to have the dns record deleted when the container is stopped. Defaults to `false`.

## Multiple Domains

If you need multiple records pointing to your public IP address you can create CNAME records in CloudFlare.

## IPv6

If you're wanting to set IPv6 records set the envrionment variable `RRTYPE=AAAA`. You will also need to run docker with IPv6 support, or run the container with host networking enabled.

## Docker Compose

If you prefer to use [Docker Compose](https://docs.docker.com/compose/):

```yml
version: '2'
services:
  cloudflare-ddns:
    image: oznu/cloudflare-ddns:latest # change 'latest' to 'armhf' or 'aarch64' if running on an arm device
    restart: always
    environment:
      - API_TOKEN=xxxxxxx
      - ZONE=example.com
      - ZONE_ID=xxxxxxx
      - SUBDOMAIN=subdomain
      - PROXIED=false
```
