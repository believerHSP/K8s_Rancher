# K8s_Topics

Monitoring related error resolved: https://github.com/rancher/rancher/issues/44511
Root cause: CRDs are older and not validating resources.

# Postgres Replication:
https://www.postgresql.fastware.com/postgresql-insider-ha-str-rep#:~:text=In%20PostgreSQL%2C%20there%20are%20two,units%20of%20tables%20and%20databases.

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


