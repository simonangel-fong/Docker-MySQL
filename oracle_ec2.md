# Install Oracle Database via EC2

---

## Pre-install

- update host file

```sh
sudo nano /etc/hosts
# add
# 18.212.132.197  ol7-19.localdomain  ol7-19
```

- correct hostname in the "/etc/hostname" file.

```sh
sudo nano /etc/hostname

# update
# ol7-19.localdomain
```

---

## Automatic Setup

```sh
sudo yum install -y oracle-database-preinstall-19c
```

```sh
sudo yum install -y bc binutils


sudo yum install -y bc binutils compat-libcap1 compat-libstdc++-33 dtrace-utils elfutils-libelf elfutils-libelf-devel fontconfig-devel glibc glibc-devel ksh libaio libaio-devel libdtrace-ctf-devel libXrender libXrender-devel libX11 libXau libXi libXtst libgcc librdmacm-devel libstdc++ libstdc++-devel libxcb make net-tools nfs-utils python python-configshell python-rtslib python-six targetcli smartmontools sysstat unixODBC

# Added by me.
sudo dnf install -y gcc
```

- Create the new groups and users.

```sh
sudo groupadd -g 54321 oinstall
sudo groupadd -g 54322 dba
sudo groupadd -g 54323 oper
sudo useradd -u 54321 -g oinstall -G dba,oper oracle

```

- Set the password for the "oracle" user.

```sh
sudo passwd oracle
# welcome
```

- editing the "/etc/selinux/config" file,

```sh
sudo nano /etc/selinux/config
# SELINUX=permissive
```

- If you have the Linux firewall enabled, you will need to disable or configure it, as shown here. To disable it, do the following.

```sh

# systemctl stop firewalld
# systemctl disable firewalld
```

- Create the directories in which the Oracle software will be installed.

```sh
sudo mkdir -p /u01/app/oracle/product/19.0.0/dbhome_1
sudo mkdir -p /u02/oradata
sudo chown -R oracle:oinstall /u01 /u02
sudo chmod -R 775 /u01 /u02
```

- Create a "scripts" directory.

```sh
sudo mkdir /home/oracle/scripts
```

- Create an environment file called "setEnv.sh".

```sh
sudo nano /home/oracle/scripts/setEnv.sh


# # Oracle Settings
# export TMP=/tmp
# export TMPDIR=$TMP

# export ORACLE_HOSTNAME=ol7-19.localdomain
# export ORACLE_UNQNAME=cdb1
# export ORACLE_BASE=/u01/app/oracle
# export ORACLE_HOME=$ORACLE_BASE/product/19.0.0/dbhome_1
# export ORA_INVENTORY=/u01/app/oraInventory
# export ORACLE_SID=cdb1
# export PDB_NAME=pdb1
# export DATA_DIR=/u02/oradata

# export PATH=/usr/sbin:/usr/local/bin:$PATH
# export PATH=$ORACLE_HOME/bin:$PATH

# export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib
# export CLASSPATH=$ORACLE_HOME/jlib:$ORACLE_HOME/rdbms/jlib
```

- /home/oracle/.bash_profile

```sh
sudo nano /home/oracle/.bash_profile

# to the end

# . /home/oracle/scripts/setEnv.sh

```


- /home/oracle/scripts/start_all.sh

```sh
sudo nano /home/oracle/scripts/start_all.sh

# #!/bin/bash
# . /home/oracle/scripts/setEnv.sh

# export ORAENV_ASK=NO
# . oraenv
# export ORAENV_ASK=YES

# dbstart \$ORACLE_HOME
```

```sh
sudo nano /home/oracle/scripts/stop_all.sh

# #!/bin/bash
# . /home/oracle/scripts/setEnv.sh

# export ORAENV_ASK=NO
# . oraenv
# export ORAENV_ASK=YES

# dbshut \$ORACLE_HOME

```


```sh
sudo chown -R oracle:oinstall /home/oracle/scripts
sudo chmod u+x /home/oracle/scripts/*.sh

sudo chmod u+x /home/oracle/scripts/setEnv.sh
sudo chmod u+x /home/oracle/scripts/start_all.sh
sudo chmod u+x /home/oracle/scripts/stop_all.sh
```

```sh
~/scripts/start_all.sh
~/scripts/stop_all.sh
```

- lonin as oracle

```sh
ssh -i "Argus_Lab.pem" oracle@ec2-18-212-132-197.compute-1.amazonaws.com
```