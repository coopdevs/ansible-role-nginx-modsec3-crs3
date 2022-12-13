# nginx_modsec3_crs role
## Ansible role for Installing Nginx, compiling ModSecurity3, and installing the OWASP CRS v3 ruleset 

Forked from @perryk [ansible-role-nginx-modsec3-crs3](https://github.com/perryk/ansible-role-nginx-modsec3-crs3)

There are a number of libraries and packages which ModSecurity3 depends on and will be installed via this role.

This role will additionally install any compilers and other build tools required for compilation. It will then remove these tools if they were not previously installed. 

Nginx support is primarily provided by the dependent role `ansible-role-nginx` by jdauphant.

https://github.com/jdauphant/ansible-role-nginx

:warning:  jdauphant's nginx role is no longer mantained.

## Requirements

Before running a playbook which calls this role:

Install any required [Ansible](https://www.ansible.com) roles from `requirements.yml` View [here](requirements.yml).

```bash
ansible-galaxy install -r requirements.yml
```
i.e this in the requirements.yml file for your project's playbook (not the requirements.yml file for this role) you will need to include both this role and the role mentioned above like this:

```yml
- src: coopdevs.nginx_modsec3_crs

- src: jdauphant.nginx
  version: v2.21.2
```

## Role Variables

Browse the role's [defaults/main.yml](defaults/main.yml) and [vars/main.yml](vars/main.yml) files to see if there is anything you would like to change or need to override by setting in your playbook.

Specific-role vars are explained below, with their default value set.

```yaml
# Enables the modsecurity compilation, installation and configuration if it is not installed
nginx_modsec3_enabled: True
 # Set the ruleset version
nginx_modsec3_crs_version: v3.4/dev
# Force modsecurity task despite it is already installed
nginx_modsec3_crs3_force_compile: False
# Enable the block mode (if False, then "Detection Only" mode is set)
nginx_modsec3_crs3_block_mode: True
```

There are lots of variables more in the nginx role, perhaps the best explanation of these are all the examples in the role [README.md](https://github.com/jdauphant/ansible-role-nginx/blob/master/README.md) file.


## Example Playbook

Example playbook calling the role adding and enabling ModSecurity for the default Nginx site.

```yaml
- hosts: servers

  vars:

    nginx_pkgs:
      - nginx
    nginx_install_epel_repo: false
    nginx_official_repo: true
    nginx_official_repo_mainline: true
    nginx_module_configs:
      - ngx_http_modsecurity_module
      - ngx_http_geoip2_module
    nginx_modules_disable:
      - ngx_http_geoip_module
    nginx_sites:
      default:
       - ...
       - modsecurity on;
       - modsecurity_rules_file /etc/nginx/modsec/main.conf;
       - ...
  roles:
    - coopdevs.nginx_modsec3_crs
```

# License

GPL-3.0-or-later

## Author Information

Perry Kollmorgen - https://github.com/perryk  
Coopdevs - https://coopdevs.org

