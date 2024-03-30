# Playbook - Ansible

## Ansible Playbook

1. Replace the <mark style="color:red;">`hosts:`</mark> option to <mark style="color:red;">**localhost**</mark> or other <mark style="color:red;">**host**</mark> of your choice.

```yaml
---

- name: Install Wordpress
  hosts: localhost
  become: true
  any_errors_fatal: true
  max_fail_percentage: 0

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: System update
      apt:
        update_cache: yes
      changed_when: false
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Dist upgrade
      apt:
        upgrade: dist
      register: upgrade_output
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Install required packages
      apt:
        name:
          - apt-transport-https
          - ca-certificates
          - software-properties-common
          - curl
        state: present
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Add Docker APT key to trusted.gpg.d
      shell: "curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg"
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Add Docker APT repository
      apt_repository:
        repo: deb [arch=amd64] https://download.docker.com/linux/ubuntu {{ ansible_lsb.codename }} stable
        state: present
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Install Docker
      apt:
        name: docker-ce
        state: latest
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Download and Install Docker Compose
      get_url:
        url: https://github.com/docker/compose/releases/latest/download/docker-compose-Linux-x86_64
        dest: /usr/bin/docker-compose
        mode: '0755'
      when: ansible_distribution in ['Ubuntu', 'Debian']

    - name: Pull the latest WordPress image
      command: docker pull wordpress:latest

    - name: Create WordPress Directory
      file:
        path: /opt/wordpress
        state: directory

    - name: Create docker-compose.yml File
      copy:
        content: |
          version: '3.9'
          services:
            wordpress:
              image: wordpress:latest
              ports:
                - "8080:80"
              restart: always
              environment:
                WORDPRESS_DB_HOST: db:3306
                WORDPRESS_DB_NAME: your_wordpress_db_name
                WORDPRESS_DB_USER: your_wordpress_db_user
                WORDPRESS_DB_PASSWORD: your_wordpress_db_password

            db:
              image: mysql:5.7
              volumes:
                - db_data:/var/lib/mysql
              restart: always
              environment:
                MYSQL_ROOT_PASSWORD: your_mysql_root_password
                MYSQL_DATABASE: your_wordpress_db_name
                MYSQL_USER: your_wordpress_db_user
                MYSQL_PASSWORD: your_wordpress_db_password

          volumes:
            db_data:
        dest: /opt/wordpress/docker-compose.yml

    - name: Start Docker Containers for WordPress and MySQL
      command: docker-compose up -d
      args:
        chdir: /opt/wordpress
```
