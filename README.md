# Ansible Role: docker_tls

An ansible Role that enables TLS support, along with geerlingguy.docker role.

## Requirements

- Ubuntu 18.04 or above
- Debian 10 or above

## Role Variable

Available variables are listed below, along with default values (see `defaults/main.yml`):

```yml
docker_tls_basepath: '/etc/docker'
docker_tls_keys:
  ca_cert: ""
  server_cert: ""
  server_key: ""
```

`docker_tls_basepath` is a directory that manages where the private key and certification files locate. `docker_tls_keys` is a dictionary that contains three keys, `ca_cert` (CA certification), `server_cert` (the Docker daemon certification), and `server_key` (the Docker daemon private key). These keys' values must be in PEM format.

```yml
docker_daemon_tls_options:
  tlsverify: true
  tlscacert: "{{ docker_tls_basepath }}/ca.crt"
  tlscert: "{{ docker_tls_basepath }}/server.crt"
  tlskey: "{{ docker_tls_basepath }}/server.key"
  hosts: ["tcp://0.0.0.0:2376", "fd://"]
```

`docker_daemon_tls_options` contains the options of dockerd relevant TLS or so. It is merged with `docker_daemon_options` of `geerlingguy.docker` and converted into `/etc/docker/daemon.json`. Note that this role installs `docker.service` drop-in file for systemd to avoid the conflict of dockerd options, `hosts` between systemd and `/etc/docker/daemon.json`. You need to configure the `hosts` option via `docker_daemon_options` or this dictionary (I recommend this dictionary to evade problems while running ansible-playbook).

## Dependencies

- geerlingguy.docker

## Example Playbook

```yml
- hosts: all

  roles:
    - geerlingguy.docker
    - poppen.docker_tls
```

## License

MIT

## Future Plan

- Merge with geerlingguy.docker (Need to support the RedHat family. PRs are always welcome).

## Author Information

Shinsuke MATSUI
