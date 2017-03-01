# Ansible playbook for installing Open-MBEE

The playbook installs JPL's Open-MBEE project on Redhat
Linux 7.0 servers. The playbook is executed by:

`ansible-playbook openmbee/site.yml -K --ask-vault-pass -u <remoteuserid>`

You will be prompted for the 'sudo' password (-K), and also by the
vault password (--ask-vault-pass), before the playbook will run.

Reference: http://docs.ansible.com/ansible/playbooks.html

## Vault

Passwords for the installation are stored in ansible 'vault'
files. For each role, there is a group_vars directory with two
files. The first file is called 'vars' and contains the variables for
that role. Each password is referenced in that file by a variable
prefixed by 'vault_'. The other file in that directory, 'vault', will
contain those 'vault_*' variables with their values and are encrypted. To
edit a vault file, run the command:

`ansible-vault edit openmbee/group_vars/tomcat-servers/vault`

Reference: http://docs.ansible.com/ansible/playbooks_vault.html

## Inventory

There are 4 host groups that need to be configured in the inventory:

* db-servers
* tomcat-servers
* msg-servers
* backup-servers

At present, each host in the tomcat-servers group will only contact the first
server in the db-servers group, so the assumption is there is only 1 host in the
db-servers group, until replication is implemented

Similarly, only one message server is configured in the tomcat hosts, so the current
playbook is assuming 1 host in the msg-servers group.

In addition, the configuration with multiple tomcat-servers to one db
has never been tested so even though this playbook will configure
that, it may not work.

### Host Variables required

Machines can have firewalls controlled by either 'firewalld' or 'iptables'. Specify
which is used as the host variable 'firewall'. Example:

      ghostbusters.hollywood.com firewall=iptables
      lotr.newzealand.org firewall=firewalld

The interface that the services shall bind to is in the host variable 'iface'. Example:

      deadpool.haha.tz iface=127.0.0.1 firewall=iptables
      
Host variables are usually in the inventory/hosts file.

Reference: http://docs.ansible.com/ansible/intro_inventory.html

## TODO

* Install certificates and configure encrypted connections on db, tomcat, activemq
* Double-check permissions of directories and files, ensure they are protected

## Roles

* common
* postgresql
* jre
* tomcat
* alfresco
* openmbee
* activemq
* backups

