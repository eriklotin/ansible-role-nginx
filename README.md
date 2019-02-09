# Ansible role: eriklotin.nginx
This role installs Docker and run nginx-proxy and nginx-proxy-letsencrypt containers.

After that you can run you own containers with environments variables:
- `VIRTUAL_HOST` - your host (example.com).
- `VIRTUAL_PORT` - port that your app is listen to 
(you should not expose ports from container with `-p` flag!).
- `LETSENCRYPT_HOST` - your domain one more time.
- `LETSENCRYPT_EMAIL` - your valid email address.

Virtual host and certificates will be created automatically.


# Install
```
ansible-galaxy install eriklotin.nginx
```
or install locally:
```yaml
ansible-galaxy install eriklotin.nginx -p ./roles/
```

# Variables

List of system users which will be added to docker system group.
```yaml
docker_users: []
```

Working directory. `nginx` in home directory by default. Will created by role.
```yaml
nginx_workdir: "{{ ansible_env.HOME }}/nginx"
```

# Example playbook

```yaml
- hosts: all

  vars:
    - docker_users: ["ubuntu"]

  roles:
    - role: eriklotin.nginx
    
  tasks:
    - name: Create an app container
      docker_container:
        name: myapp
        image: myappimage
        state: started
        restart_policy: on-failure
        restart_retries: 5
        restart: no
        env:
          VIRTUAL_HOST: "example.com"
          VIRTUAL_PORT: "3000"
          LETSENCRYPT_HOST: "example.com"
          LETSENCRYPT_EMAIL: "admin@example.com"
```

# Dependencies

This role uses [eriklotin.docker](https://github.com/eriklotin/ansible-role-docker) role to install Docker 
and do post-install tasks.


# License
MIT. See `LICENSE` file.

# Author
Created in 2019-02 by [Erik Lotin](https://github.com/eriklotin).