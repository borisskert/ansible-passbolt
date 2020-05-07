# ansible-passbolt

[Ansible](https://www.ansible.com/) role to install and setup [passbolt](https://www.passbolt.com/) as [Docker](https://www.docker.com/) container managed by [systemd](https://systemd.io). 

## System requirements

* Systemd
* Docker
* docker-compose

## Role requirements

* python-docker package

## What this role does

* Create volumes
* Generate database setups
* Setup docker-compose file
* Pull Docker images:
  * [passbolt](https://hub.docker.com/r/passbolt/passbolt/)
  * [mariadb](https://hub.docker.com/_/mariadb/)
* Setup systemd service

## Role parameters

| Variable       | Type | Mandatory? | Default | Description                  |
|----------------|------|------------|---------|------------------------------|
| http_port      | port number | no  | 80      | Listen web port              |
| https_port     | port number | no  | 443     | Listen ssl port              |
| interface      | network address | no | 0.0.0.0 | Mapped network published ports |
| secret_size    | number          | no | 16      | Size of the generated database passwords |
| full_base_path | url             | yes |        | Passbolt's APP_FULL_BASE_URL: Passbolt base url |
| passbolt_volume | absolute path  | yes |        | Directory where the persistent data will be stored |
| image_version   | text           | no  | `latest` | Used Passbolt docker image version               |
| db_image_version | text          | no  | `latest` | Used MariaDB docker image version                |
| passbold_key_length | number     | no  | 2048     | Size of the generated keys                       |
| passbold_subkey_length | number  | no  | 2048     | Size of the generated subkeys                    |
| passbold_key_name      | text    | no  |          | Used name of the generated key                   |
| passbold_key_email     | email   | no  |          | Used email of the generated keys                 |
| email_from             | email   | no  |          | Used email for sent emails                       |
| email_host             | hostname | no |          | Used SMTP host for sending mails                 |
| email_port             | port number | no |       | Used SMTP port for sending mails                 |
| email_username         | text        | no |       | Used username for sending mails                  |
| email_password         | text        | no |       | Used password for sending mails                  |
| email_tls              | boolean     | no | no    | Using TLS for sending mails?                     |
| public_registration    | boolean     | no | no    | Enable public registration for this server       |

## Usage

### Add to `requirements.yml`

```yaml
- name: install-passbolt
  src: https://github.com/borisskert/ansible-passbolt.git
  scm: git
```

### Minimal `playbook.yml`

```yaml
- hosts: test_machine
  become: yes

  roles:
    - role: install-passbolt
      passbolt_volume: /srv/passbolt
      full_base_path: http://192.168.33.83:80
```

### Typical `playbook.yml`

```yaml
- hosts: test_machine
  become: yes

  roles:
    - role: install-passbolt
      image_version: 2.12.1-debian
      db_image_version: 10.4
      http_port: 10080
      https_port: 10443
      interface: 0.0.0.0
      passbolt_volume: /srv/passbolt
      secret_size: 32
      full_base_path: http://192.168.33.83:10080
      passbold_key_length: 4096
      passbold_subkey_length: 4096
      passbold_key_name: my key name
      passbold_key_email: my@fakemail.com
      email_from: my@fakemail.com
      email_host: smtp.fakemail.com
      email_port: 587
      email_username: fakeuser
      email_password: fakepass
      email_tls: yes
      public_registration: yes
```

## Further links

* [passbolt @ Github](https://github.com/passbolt)
* [passbolt @ Docker Hub](https://hub.docker.com/r/passbolt/passbolt/)
* [passbolt_docker @ Github](https://github.com/passbolt/passbolt_docker)

## License

[The MIT License (MIT)](LICENSE.txt)
