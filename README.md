# K8s_Topics

Monitoring related error resolved: https://github.com/rancher/rancher/issues/44511
Root cause: CRDs are older and not validating resources.

# Postgres Replication:
https://www.postgresql.fastware.com/postgresql-insider-ha-str-rep#:~:text=In%20PostgreSQL%2C%20there%20are%20two,units%20of%20tables%20and%20databases.

Administation commands for PG DB servers: https://www3.ntu.edu.sg/home/ehchua/programming/sql/PostgreSQL_GetStarted.html

#Script for Runnning 2 postgres containers i.e. master & slave and also creating a podman network: 
#!/bin/bash

podman network create --subnet 172.22.0.0/16 pg


podman run -d \
        --name master \
        --network pg \
        --ip 172.22.0.2 \
        --restart always \
        -v /home/lenovo/scratch/replication/master:/var/lib/postgresql/data \
        -p 5434:5432 \
        -e POSTGRES_PASSWORD=adminm@1234 \
        -e POSTGRES_USER=adminm \
        -e POSTGRES_DB=test_db_master \
          postgres:15.6


podman run -d \
        --name slave \
        --network pg \
        --ip 172.22.0.3 \
        --restart always \
        -v /home/lenovo/scratch/replication/slave:/var/lib/postgresql/data \
        -p 5435:5432 \
        -e POSTGRES_PASSWORD=admins@1234 \
        -e POSTGRES_USER=admins \
        -e POSTGRES_DB=test_db_slave \
          postgres:15.6



#Login to my test_db_master
root@ff1e0eb14a1e:/# psql -d test_db_master -U adminm -W                                
Password: 
psql (15.6 (Debian 15.6-1.pgdg120+2))
test_db_master=# 

#Environment variables of Database server:
lenovo@ipa:~/scratch/replication/scripts$ podman exec -it 41989b32abd2 bash
root@41989b32abd2:/# 
root@41989b32abd2:/# env
POSTGRES_PASSWORD=admins@1234
PWD=/
container=podman
HOME=/root
LANG=en_US.utf8
GOSU_VERSION=1.17
PG_MAJOR=15
PG_VERSION=15.6-1.pgdg120+2
TERM=xterm
SHLVL=1
POSTGRES_USER=admins
PGDATA=/var/lib/postgresql/data
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/lib/postgresql/15/bin
POSTGRES_DB=test_db_slave
_=/usr/bin/env
root@41989b32abd2:/# 


