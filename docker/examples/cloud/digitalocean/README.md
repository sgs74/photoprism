PhotoPrism: Open-Source Photo Management 1-Click App
====================================================

DESCRIPTION
----------------------------------------------------

PhotoPrism® is a privately hosted app for browsing, organizing, and sharing your photo collection. It makes use of the latest technologies to tag and find pictures automatically without getting in your way. Say goodbye to solutions that force you to upload your visual memories to the cloud!

SOFTWARE INCLUDED
----------------------------------------------------

- [PhotoPrism latest](https://docs.photoprism.org/release-notes/), AGPL 3
- [Docker CE 20.10](https://docs.docker.com/engine/release-notes/), Apache 2
- [Traefik 2.4](https://github.com/traefik/traefik/releases), MIT
- [MariaDB 10.5](https://mariadb.com/kb/en/release-notes/), GPL 2
- [Ofelia 0.3.4](https://github.com/mcuadros/ofelia/releases), MIT
- [Watchtower 1.3](https://github.com/containrrr/watchtower/releases), Apache 2

GETTING STARTED
----------------------------------------------------

It may take a few minutes until your Droplet is provisioned and all services have been initialized. You may then access your instance by opening the following URL in a Web browser (see "Using Let's Encrypt HTTPS" for how to get a valid certificate):

```
https://YOUR-SERVER-IP/
```

You'll see the initial admin password when running

```
cat /root/.initial-password.txt
```

as root on your server. To open a terminal:

```
ssh root@YOUR-SERVER-IP
```

Data and all config files related to PhotoPrism can be found in

```
/opt/photoprism
```

The main docker-compose config file for changing config options is

```
/opt/photoprism/docker-compose.yml
```

The server is running as "photoprism" (UID 1000) by default.

## System Requirements ##

We recommend running PhotoPrism on a server with at least 2 cores and 4 GB of memory. Indexing and searching may be slow on smaller Droplets, depending on how many and what types of files you upload.

## Using Let's Encrypt HTTPS ##

By default, a self-signed certificate will be used for HTTPS connections. Browsers are going to show a security warning because of that. Depending on your settings, they may also refuse connecting at all.

To get an official, free HTTPS certificate from Let's Encrypt, your server needs a fully qualified public domain name, e.g. "photos.yourdomain.com".

You may add a static DNS entry (on DigitalOcean go to Networking > Domains) for this, or use a Dynamic DNS service of your choice.

Once your server has a public domain name, please disable the self-signed certificate and enable domain based routing in docker-compose.yml and traefik.yaml (see inline instructions in !! UPPERCASE !!):

```
ssh root@YOUR-SERVER-IP
cd /opt/photoprism
nano docker-compose.yml
nano traefik.yaml
```

Then restart services in a terminal for the changes to take effect:

```
docker-compose stop
docker-compose up -d
```

You should now be able to access your instance without security warnings:

```
https://photos.yourdomain.com/
```

Note the first request may still fail while Traefik gets and installs the new certificate. Try again after 30 seconds.
