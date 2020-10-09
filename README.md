# Ansible Role: Docker

An Ansible Role that installs [Docker](https://www.docker.com) on Oracle Linux.

## Requirements

None.

## Role Variables

Available variables are listed below, along with default values (see `defaults/main.yml`):

    docker_package: "docker-engine"
    docker_package_state: present

You can also specify a specific version of Docker to install using the distribution-specific format: OracleLinux: `docker-engine-<VERSION>`.

You can control whether the package is installed, uninstalled, or at the latest version by setting `docker_package_state` to `present`, `absent`, or `latest`, respectively. Note that the Docker daemon will be automatically restarted if the Docker package is updated. This is a side effect of flushing all handlers (running any of the handlers that have been notified by this and any other role up to this point in the play).

    docker_service_state: started
    docker_service_enabled: true
    docker_restart_handler_state: restarted

Variables to control the state of the `docker` service, and whether it should start on boot. If you're installing Docker inside a Docker container without systemd or sysvinit, you should set these to `stopped` and set the enabled variable to `no`.

    docker_install_compose: true
    docker_compose_version: "1.26.0"
    docker_compose_path: /usr/local/bin/docker-compose

Docker Compose installation options.

    docker_users:
      - user1
      - user2

A list of system users to be added to the `docker` group (so they can use Docker on the server).

## Use with Ansible (and `docker` Python library)

Many users of this role wish to also use Ansible to then _build_ Docker images and manage Docker containers on the server where Docker is installed. In this case, you can easily add in the `docker` Python library using the `geerlingguy.pip` role:

```yaml
- hosts: all

  vars:
    pip_install_packages:
      - name: docker

  roles:
    - geerlingguy.pip
    - lorf.docker-ol
```

## Dependencies

None.

## Example Playbook

```yaml
- hosts: all
  roles:
    - lorf.docker-ol
```

## License

MIT / BSD

## Author Information

This role was created in 2017 by [Jeff Geerling](https://www.jeffgeerling.com/), author of [Ansible for DevOps](https://www.ansiblefordevops.com/).
