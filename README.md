# Site24x7 Agent Dockerfile

This repository is meant to build the base image for a Site24x7 Agent container. You will have to use the resulting image to configure and run the Agent.


## Quick Start

Run the below command in your server to monitor the host via site24x7-agent container

```
docker run -d --name site24x7-agent \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v /proc/:/host/proc/:ro \
  -v /sys:/host/sys/:ro \
  -e KEY=<device_key> \
  mes24x7/betaagent:latest
```

## Configuration


### Hostname

By default the agent container will use the `Name` field found in the `docker info` command from the host as a hostname. To change this behavior you can update the `hostname` field in `/opt/site24x7/monagent/conf/monagent.cfg`. The easiest way for this is to use the `HOSTNAME` environment variable (see below).

```
docker run -d --name site24x7-agent \
  -v /var/run/docker.sock:/var/run/docker.sock:ro \
  -v /proc/:/host/proc/:ro \
  -v /sys:/host/sys/:ro \
  -e KEY=<device_key> \
  -e HOSTNAME=<host_name> \
  mes24x7/betaagent:latest
```

## Limitations

The Agent won't be able to collect disk metrics from volumes that are not mounted to the Agent container. If you want to monitor additional partitions, make sure to share them to the container in your docker run command (e.g. `-v /data:/data:ro`)

Docker isolates containers from the host. As a result, the Agent won't have access to all host metrics.


Also, several integrations might be incomplete. See the "Contribute" section.


## Contribute

If you notice a limitation or a bug with this container, feel free to open a [Github issue](https://github.com/DataDog/docker-dd-agent/issues). If it concerns the Agent itself, please refer to its [documentation](https://docs.datadoghq.com/) or its [wiki](https://github.com/DataDog/dd-agent/wiki).
