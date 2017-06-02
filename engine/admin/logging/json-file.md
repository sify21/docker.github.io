---
description: Describes how to use the json-file logging driver.
keywords: json-file, docker, logging, driver
redirect_from:
- /engine/reference/logging/json-file/
title: JSON File logging driver
---


By default, Docker captures the standard output (and standard error) of all your containers,
and writes them in files using the JSON format. The JSON format annotates each line with its
origin (`stdout` or `stderr`) and its timestamp. Each log file containers information about
only one container.

## Usage

Docker uses the JSON File logging driver by default. To explicitly set it,
use the `--log-driver` and `--log-opt` flags when starting Docker:

```bash
dockerd
  -–log-driver json-file –-log-opt max-size=10m
```

To make the change persistent, configure it in `/etc/docker/daemon.json`:

```json
{
  "log-driver": "json-file",
  "log-opts":  {
    "max-size": "10m"
  }
}
```

You can set the logging driver for a specific container by using the
`--log-driver` flag to `docker create` or `docker run`:

```bash
$ docker run \
      -–log-driver json-file –-log-opt max-size=10m \
      alpine echo hello world
```

### Options

The `json-file` logging driver supports the following logging options:

| Option     | Description              | Example  value                            |
|------------|--------------------------|-------------------------------------------|
| `max-size` | The maximum size of the log before it is rolled. A positive integer plus a modifier representing the unit of measure (`k`, `m`, or `g`). | `--log-opt max-size=10m` |
| `max-file` | The maximum number of log files that can be present. If rolling the logs creates excess files, the oldest file is removed. **Only effective when `max-size` is also set.** A positive integer. | `--log-opt max-file=3` |
| `labels`   | Applies when starting the Docker daemon. A comma-separated list of logging-related labels this daemon will accept. Used for advanced [log tag options](log_tags.md).| `--log-opt labels=production_status,geo` |
| `env`      | Applies when starting the Docker daemon. A comma-separated list of logging-related environment variables this daemon will accept. Used for advanced [log tag options](log_tags.md). | `--log-opt env=os,customer` |

> **Note**: If `max-size` and `max-file` are set, `docker logs` only returns the
> log lines from the newest log file.

### Examples

This example starts an `alpine` container which can have a maximum of 3 log
files no larger than 10 megabytes each.

```bash
$ docker run -it --log-opt max-size=10m --log-opt max-file=3 alpine ash
```