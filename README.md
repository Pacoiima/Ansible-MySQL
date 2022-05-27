# Ansible-MySQL
Instalar Mysql por medio de ansible

Como primer paso instalamos Ansible
- sudo yum install ansible

Configuramos los hosts
- sudo vi /etc/ansible/hosts
		[server]
		"alias" ansible_ssh_host="Server - IP"
		
Creamos el archivo yml 
---
- name: Instalar MySQL
  hosts: Master
  vars: 
    mysql_root_password: Montechelo2022*
  become: yes
  tasks:
      - name: Instalar wget
        yum: name=wget state=installed

      - name: Instalar Python
        yum: name=MySQL-python state=installed

      - name: Instalar llaves
        get_url: url=https://repo.mysql.com/RPM-GPG-KEY-mysql dest=/etc/pki/rpm-gpg/RPM-GPG-KEY-mysql

      - name: Descargar libreria MySQL
        get_url: 
          url: https://repo.mysql.com/mysql80-community-release-el7-6.noarch.rpm 
          dest: /usr/mysql/mysql80-community-release-el7-6.noarch.rpm

      - name: Instalar el rpm
        yum: 
          name: /usr/mysql/mysql80-community-release-el7-6.noarch.rpm 
          state: present

      - name: Instalar MySQL
        yum: name=mysql-server state=installed
				
Nos saldra un error por configuracion de usuario
- Ingresamos a nuestra VM por medio de ssh
	Luego ingresamos a mysql y escribimos el siguiente comando
	- "ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'Contraseña';"
	- flush privileges;
	- exit

Si nos pide crear el archivo .my.cnf en la locacion /root/ le ingresamos las credenciales
  [mysqld]
	user=root
	password="contraseña"
