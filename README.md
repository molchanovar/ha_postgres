# ha_postgres
HA-Postgres cluster on Patroni (haproxy + etcd)

### Usefull cmd: 
```
ansible -i inventory -m ping <host_name>
ansible-playbook -i inventory <role_name.yml> -t <tag_name> -v

pg_lsclusters
systemctl status patroni
systemctl status postgresql
patronictl -c /etc/patroni.yml list
```
