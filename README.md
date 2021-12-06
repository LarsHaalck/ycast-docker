# YCast Docker Container

Forked from [MaartenSanders/ycast-docker](https://github.com/MaartenSanders/ycast-docker).

## Configuration
See [milaq/YCast](https://github.com/milaq/YCast) for configuration options.

### Adding stations
Stations can be added to a `stations.yml` as described in the in [milaq/YCast](https://github.com/milaq/YCast).
The `docker-compose.yml` mounts the file to the correct path, so the YCast container can pick it up.


## Dockerhub
See [larshaalck/ycast](https://hub.docker.com/r/larshaalck/ycast) for images for `linux/amd64`, `linux/arm/v7` and `linux/arm64`.


## Running next to Pi-Hole

When running on a Raspberry-Pi together with a Pi-Hole Container (e.g. the official one from [here](https://github.com/pi-hole/docker-pi-hole)),
changing the port of YCast is recommended to not interfere with the Pi-Hole Interface.

This can be done in the supplied `docker-compose.yml` by changing the `ports` lines to:

```yml
[...]
ports:
  - 8000:80
[...]
```
YCast now listens on port 8000. In order for your receiver to find YCast, add a proxy config to the lighttpd instance supplied by the Pi-Hole container.

20-ycast.conf:
```
server.modules   += ( "mod_proxy" )

#YCAST
$HTTP["host"] =~ "vtuner.com" {
        proxy.server = ( "" =>  ( (
                                "host" => "<IP-Address-of-the-pi>",
                                "port" => "8000",
                                 ) ) )
        proxy.forwarded = (
                        "for"          => 1,
                        "proto"        => 1,
                        "host"         => 1,
    )

}
```

and then add a volume line to the `docker-compose.yml` of your Pi-Hole:

```yml
[...]
volumes:
      - './20-ycast.conf:/etc/lighttpd/conf-enabled/20-ycast.conf'
[...]
```

After restarting the Pi-Hole, entering vtuner.com should lead to the YCast website.
Don't forget to enter local DNS records in your Pi-Hole or your router, so that vtuner.com is redirected to your raspberry pi.
