# gitea_runner

This Ansible role installs and configures a gitea Act Runner for executing GitHub Actions-compatible workflows.

## Installation

```bash
ansible-galaxy install volker-raschek.gitea_runner
```

## Usage

### Simple Playbook

```yaml
- hosts: runners
  become: yes
  roles:
    - role: volker-raschek.gitea_runner
      vars:
        gitea_runner_gitea_url: "https://your-gitea-instance.com"
        gitea_runner_token: "your-registration-token"
```

### Advanced Configuration

```yaml
- hosts: runners
  become: yes
  roles:
    - role: volker-raschek.gitea_runner
      vars:
        gitea_runner_gitea_url: "https://your-gitea-instance.com"
        gitea_runner_token: "your-registration-token"
        gitea_runner_config:
          runner:
            capacity: 2
            labels:
              - "ubuntu-latest:docker://docker.gitea.com/runner-images:ubuntu-latest"
              - "custom-label:docker://custom-image:latest"
          container:
            privileged: true
```

## Further ansible roles

This ansible role is used in combination with other ansible roles of `volker-raschek`. You can search for the other
ansible roles via the following command.

```bash
$ ansible-galaxy role search --author "volker-raschek"

Found roles matching your search:

 Name                      Description
 ----                      -----------
 volker-raschek.bind9      Role to install and configure bind9 on different distributions
 volker-raschek.dhcpd      Role to install and configure dhcpd on different distributions
 volker-raschek.renovate   Role to configure renovate as container image
 ...
```

## Parameters

### Act Runner

| Name                                           | Description                                                                      | Value                                                                                                                                                                                                             |
| ---------------------------------------------- | -------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `gitea_runner_config_file`                     | Path to the gitea_runner configuration file                                      | `/etc/gitea_runner/config.yaml`                                                                                                                                                                                   |
| `gitea_runner_config.log.level`                | The level of logging, can be trace, debug, info, warn, error, fatal              | `info`                                                                                                                                                                                                            |
| `gitea_runner_config.runner.file`              | Where to store the registration result                                           | `.runner`                                                                                                                                                                                                         |
| `gitea_runner_config.runner.capacity`          | Execute how many tasks concurrently at the same time                             | `1`                                                                                                                                                                                                               |
| `gitea_runner_config.runner.envs`              | Extra environment variables to run jobs                                          | `{}`                                                                                                                                                                                                              |
| `gitea_runner_config.runner.env_file`          | Extra environment variables to run jobs from a file                              | `.env`                                                                                                                                                                                                            |
| `gitea_runner_config.runner.timeout`           | The timeout for a job to be finished                                             | `3h`                                                                                                                                                                                                              |
| `gitea_runner_config.runner.shutdown_timeout`  | The timeout for the runner to wait for running jobs to finish when shutting down | `0s`                                                                                                                                                                                                              |
| `gitea_runner_config.runner.insecure`          | Whether skip verifying the TLS certificate of the Gitea instance                 | `false`                                                                                                                                                                                                           |
| `gitea_runner_config.runner.fetch_timeout`     | The timeout for fetching the job from the Gitea instance                         | `5s`                                                                                                                                                                                                              |
| `gitea_runner_config.runner.fetch_interval`    | The interval for fetching the job from the Gitea instance                        | `2s`                                                                                                                                                                                                              |
| `gitea_runner_config.runner.github_mirror`     | The mirror address of the github that pulls the action repository                | `""`                                                                                                                                                                                                              |
| `gitea_runner_config.runner.labels`            | The labels of a runner are used to determine which jobs the runner can run       | `["ubuntu-latest:docker://docker.gitea.com/runner-images:ubuntu-latest","ubuntu-22.04:docker://docker.gitea.com/runner-images:ubuntu-22.04","ubuntu-20.04:docker://docker.gitea.com/runner-images:ubuntu-20.04"]` |
| `gitea_runner_config.cache.enabled`            | Enable cache server to use actions/cache                                         | `true`                                                                                                                                                                                                            |
| `gitea_runner_config.cache.dir`                | The directory to store the cache data                                            | `""`                                                                                                                                                                                                              |
| `gitea_runner_config.cache.host`               | The host of the cache server                                                     | `""`                                                                                                                                                                                                              |
| `gitea_runner_config.cache.port`               | The port of the cache server                                                     | `0`                                                                                                                                                                                                               |
| `gitea_runner_config.cache.external_server`    | The external cache server URL                                                    | `""`                                                                                                                                                                                                              |
| `gitea_runner_config.container.network`        | Specifies the network to which the container will connect                        | `""`                                                                                                                                                                                                              |
| `gitea_runner_config.container.privileged`     | Whether to use privileged mode when launching task containers                    | `false`                                                                                                                                                                                                           |
| `gitea_runner_config.container.options`        | Other options to be used when the container is started                           | `nil`                                                                                                                                                                                                             |
| `gitea_runner_config.container.workdir_parent` | The parent directory of a job's working directory                                | `nil`                                                                                                                                                                                                             |
| `gitea_runner_config.container.valid_volumes`  | Volumes that can be mounted to containers                                        | `[]`                                                                                                                                                                                                              |
| `gitea_runner_config.container.docker_host`    | Overrides the docker client host with the specified one                          | `""`                                                                                                                                                                                                              |
| `gitea_runner_config.container.force_pull`     | Pull docker image(s) even if already present                                     | `true`                                                                                                                                                                                                            |
| `gitea_runner_config.container.force_rebuild`  | Rebuild docker image(s) even if already present                                  | `false`                                                                                                                                                                                                           |
| `gitea_runner_config.container.require_docker` | Always require a reachable docker daemon                                         | `false`                                                                                                                                                                                                           |
| `gitea_runner_config.container.docker_timeout` | Timeout to wait for the docker daemon to be reachable                            | `0s`                                                                                                                                                                                                              |
| `gitea_runner_config.host.workdir_parent`      | The parent directory of a job's working directory                                | `nil`                                                                                                                                                                                                             |
| `gitea_runner_unix_extra_groups`               | List of additional unix groups to append the executing gitea-runner user to.     | `[]`                                                                                                                                                                                                              |
| `gitea_runner_gitea_url`                       | The URL of the Gitea instance                                                    | `""`                                                                                                                                                                                                              |
| `gitea_runner_token`                           | The registration token for the gitea_runner                                      | `""`                                                                                                                                                                                                              |

### Package Management

| Name                        | Description                                                | Value  |
| --------------------------- | ---------------------------------------------------------- | ------ |
| `gitea_runner_update_cache` | Whether to update package cache before installing packages | `true` |
