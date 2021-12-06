# YCast Docker Container

Forked from [MaartenSanders/ycast-docker](https://github.com/MaartenSanders/ycast-docker).

## Configuration
See [milaq/YCast](https://github.com/milaq/YCast) for configuration options.

### Adding stations
Stations can be added to a `stations.yml` as described in the in [milaq/YCast](https://github.com/milaq/YCast).
The `docker-compose.yml` mounts the file to the correct path, so the YCast container can pick it up.
