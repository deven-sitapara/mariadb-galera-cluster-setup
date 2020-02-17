##Readme##

NOTE: DONT USE SAME PASSWORD


Nodes: 172.16.1.173, 172.17.1.173, 172.18.1.173


----------------------------------
sudo yum -y install MariaDB-server MariaDB-client MariaDB-connect-engine

sudo systemctl start mariadb ; sudo systemctl enable mariadb

----------------------------------

sudo mysql -uroot

set password = password("WelComE.01!");

GRANT ALL PRIVILEGES ON \*.\* TO 'cluster-user'@'%' IDENTIFIED BY 'clusterpass' WITH GRANT OPTION;

GRANT PROCESS ON \*.\* TO 'clustercheck'@'localhost' IDENTIFIED BY 'clusterpass';

quit;

----------------------------------

sudo yum -y install  rsync policycoreutils-python


----------------------------------

Step 3 — Configuring the First Node

sudo vi /etc/my.cnf.d/galera.cnf

[mysqld]

binlog_format=ROW

default-storage-engine=innodb

innodb_autoinc_lock_mode=2

bind-address=0.0.0.0

# Galera Provider Configuration

wsrep_on=ON

wsrep_provider=/usr/lib64/galera-4/libgalera_smm.so

# Galera Cluster Configuration

wsrep_cluster_name="web_cluster"

wsrep_cluster_address="gcomm://172.16.1.173,172.17.1.173,172.18.1.173"


# Galera Synchronization Configuration

#wsrep_sst_method=rsync
wsrep_sst_method=mariabackup

wsrep_sst_auth=cluster-user:clusterpass



# Galera Node Configuration

wsrep_node_address="172.16.1.173"

wsrep_node_name="web-db-01"

----------------------------------

sudo service firewalld stop

sudo systemctl stop mariadb

-------------------

#On the first node, bootstrap the cluster by executing

sudo galera_new_cluster


#start serverss one by one 2nd and 3rd

sudo systemctl start  mariadb

#check status

sudo systemctl status  mariadb
