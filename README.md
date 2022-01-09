# ha_postgres
HA-Postgres cluster on Patroni (haproxy + etcd)

### Setup
```
Name 		IP-address 			Purpose
Node1 		192.168.1.11 		PostgreSQL, Patroni
Node2 		192.168.1.12 		PostgreSQL, Patroni
Haproxy 	192.168.1.13 		haproxy
Etcd  		192.168.1.14 		etcd
```

### Usefull cmd: 
```
ansible -i inventory -m ping <host_name>
ansible-playbook -i inventory <role_name.yml> -t <tag_name> -v

sudo -i -u postgres
pg_lsclusters
systemctl status patroni
systemctl status postgresql
patronictl -c /etc/patroni.yml list
```
