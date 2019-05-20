# Como usar

Para ejecutar esta instacia de Oracle EE se necesita tener instalado tanto [docker](https://docs.docker.com/install/) como [docker-compose](https://docs.docker.com/compose/install/) en nuestra máquina y tener acceso a la [imagen](https://hub.docker.com/_/oracle-database-enterprise-edition) de ORACLE EE

### Ingresar en Docker Store con nuestras credenciales

- docker login

### Clonar repositorio

- git clone https://github.com/jjuanrivvera99/oracle-ee-docker-compose.git oracle
- cd oracle

### Levantar el servicio

- docker-compose up -d
 
## Conectarse a la base de datos

###### USUARIO: SYS as SYSDBA
###### CONTRASEÑA: Oradoc_db1
###### SID: ORCLCDB

Usando SQL*Plus fuera del contenedor:

- sqlplus sys/Oradoc_db1@ORCLCDB as sysdba

Usando SQL*Plus desde el contenedor:

- docker exec -it oracle bash -c "source /home/oracle/.bashrc; sqlplus /nolog"
- SQL> connect sys as sysdba
- contraseña: Oradoc_db1

## Crear usuario con todos los privilegios
- alter session set "_ORACLE_SCRIPT"=true;
- CREATE USER username IDENTIFIED BY password;
- GRANT ALL PRIVILEGES TO username;

## Persistencia de datos
- Cambiar /my-dir por el directorio de preferencia (Este directoria endrá que tener permisos de lectura y escritura)
  
##### Ejemplo
    version: '3.7'

    services:
      oracle:
        container_name: oracle
        image: store/oracle/database-enterprise:12.2.0.1
        ports:
          - 1521:1521
        volumes:
          - type: bind
            source: /var/oracle-data #<<-----------
            target: /ORCL
