# Minimal webserver

For certain use cases, you may need a docker web server very very small. The goal? Having a curl that returns `http/200`, basic liveness probe.

But, to not overload the system, this image has to be as tiny as possible.

Thanks to Florin Lipan, based on his [blog post](https://lipanski.com/posts/smallest-docker-image-static-website), this build image is **** weight, and answers `http/200` with a JSON body:

```bash
$ docker images
REPOSITORY                    TAG                   IMAGE ID       CREATED         SIZE
arnaduga/minimalwebserver     latest                ba589bd3f0a1   6 minutes ago   154kB

$ docker run --rm -p 3001:3000 -d --name webserver arnaduga/minimalwebserver:latest
d85b0f0e2632d59656e06d17fecbde1c6526becb17bec29baa664c66aa2552c8

$ docker ps
CONTAINER ID   IMAGE                              COMMAND                  CREATED          STATUS          PORTS                    NAMES
d85b0f0e2632   arnaduga/minimalwebserver:latest   "/busybox httpd -f -…"   17 seconds ago   Up 16 seconds   0.0.0.0:3001->3000/tcp   webserver

$ curl http://localhost:3001 -w "\nReturn code: %{http_code}"
{"status":"ok"}
Return code: 200

$ docker stop webserver
```

## Build

```
$ docker build -t arnaduga/minimalwebserver:latest .

```