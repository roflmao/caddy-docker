# caddy

A [Docker](http://docker.com) image for [Caddy](http://caddyserver.com). This image includes the [git](http://caddyserver.com/docs/git) plugin.  Plugins can be configured via the `plugins` build arg.


## Getting Started

```sh
$ docker run -d -p 2015:2015 roflmao/caddy
```

Point your browser to `http://127.0.0.1:2015`.

> Be aware! If you don't bind mount the location certificates are saved to, you may hit Let's Encrypt rate [limits](https://letsencrypt.org/docs/rate-limits/) rending further certificate generation or renewal disallowed (for a fixed period)! See "Saving Certificates" below!

### Saving Certificates

Save certificates on host machine to prevent regeneration every time container starts.
Let's Encrypt has [rate limit](https://community.letsencrypt.org/t/rate-limits-for-lets-encrypt/6769).
```sh
$ docker run -d \
    -v $(pwd)/Caddyfile:/etc/Caddyfile \
    -v $HOME/.caddy:/root/.caddy \
    -p 80:80 -p 443:443 \
    roflmao/caddy
```


Here, `/root/.caddy` is the location *inside* the container where caddy will save certificates.

Additionally, you can use an *environment variable* to define the exact location caddy should save generated certificates:

```sh
$ docker run -d \
    -e "CADDYPATH=/etc/caddycerts" \
    -v $HOME/.caddy:/etc/caddycerts \
    -p 80:80 -p 443:443 \
    roflmao/caddy
```

Above, we utilize the `CADDYPATH` environment variable to define a different location inside the container for
certificates to be stored. This is probably the safest option as it ensures any future docker image changes don't interfere with your ability to save certificates!

##### Run the image

```sh
$ docker run -d -v $(pwd)/Caddyfile:/etc/Caddyfile -p 2015:2015 roflmao/caddy
```
Point your browser to `http://127.0.0.1:2015`.

## Usage

#### Paths in container

Caddyfile: `/etc/Caddyfile`

Sites root: `/srv`

#### Using local Caddyfile and sites root

Replace `/path/to/Caddyfile` and `/path/to/sites/root` accordingly.

```sh
$ docker run -d \
    -v /path/to/sites/root:/srv \
    -v path/to/Caddyfile:/etc/Caddyfile \
    -p 2015:2015 \
    roflmao/caddy
```

### Let's Encrypt Auto SSL
**Note** that this does not work on local environments.

Use a valid domain and add email to your Caddyfile to avoid prompt at runtime.
Replace `mydomain.com` with your domain and `user@host.com` with your email.
```
mydomain.com
tls user@host.com
```

##### Run the image

You can change the the ports if ports 80 and 443 are not available on host. e.g. 81:80, 444:443

```sh
$ docker run -d \
    -v $(pwd)/Caddyfile:/etc/Caddyfile \
    -p 80:80 -p 443:443 \
    roflmao/caddy
```
