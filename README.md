Ansible Role: Upsnap-docker
===========================

Install Upsnap Docker Compose project.

- https://github.com/seriousm4x/UpSnap

Requirements
------------

Requires the following to be installed:
- docker
- docker compose

Role Variables
--------------

Common system variables:

```yaml
timezone: UTC
```

Common Docker projects variables:

```yaml
# Base directory for Docker projects
docker_projects_path: # /var/apps
```

Available role variables are listed below, along with default values (see `defaults/main.yml`):

```yaml
# Docker project variables

upsnap_project_name: upsnap

# Docker project dynamic vars (uses `docker_project_name` prefix, adapt if overridden)

upsnap_traefik_router_service: upsnap
upsnap_traefik_loadbalancer_server_port: 8090
upsnap_traefik_middlewares: []
#  - "basic-auth@file"   # see djuuu.traefik_docker files/dynamic-conf/middlewares/basic-auth.yml


# Upsnap project variables

# ghcr.io/seriousm4x/upsnap image version
upsnap_version: 4

# API auth is managed by app, remove basic auth if present on app root
upsnap_api_middlewares: []

# Risky API endpoints: shutdown, scan
upsnap_risky_api_middlewares: []
#  - "internal-access@file" # see djuuu.traefik_docker templates/dynamic-conf/middlewares/internal-access.yml.j2

# Admin access
upsnap_admin_middlewares:
#  - "internal-access@file" # see djuuu.traefik_docker templates/dynamic-conf/middlewares/internal-access.yml.j2

# Interval in which the devices are pinged (UPSNAP_INTERVAL)
upsnap_interval: "*/10 * * * * *"

# Used for device discovery on local network (UPSNAP_SCAN_RANGE)
upsnap_scan_range: "192.168.1.0/24"

# Custom website title
upsnap_website_title: "Upsnap WoL"

# Useful when not running as root
# https://github.com/seriousm4x/UpSnap/issues/207
upsnap_ping_privileged: true
```

Dependencies
------------

This role depends on :
- [djuuu.docker_project](https://github.com/Djuuu/ansible-role-docker-project)

Some variables allow integration with:
- [djuuu.traefik_docker](https://github.com/Djuuu/ansible-role-traefik-docker)

Example Playbook
----------------

```yaml
- hosts: all
  gather_facts: true
  gather_subset:
    - "!all"
    - "!min"
    - user_id

  roles:
    - djuuu.upsnap_docker
```

License
-------

Beerware License
