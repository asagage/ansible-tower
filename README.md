ansible-tower
=========

This ansible role is used to setup ansible-tower. It configures tower
for LDAP authentication and it also supports upgrades and backups.


Role Variables
--------------

This role expects some vars:

`configure_ldap: true` - Do you want to enable LDAP
```
# Ldap configuration
org_name: 'YourOrg'
team_name: 'yourTeam'
ldap_url: 'ldap://dc1.yourdomain.com:389, ldap://dc2.yourdomain.com:389'
bind_dn: "{{ vaulted_bind_dn }}"
bind_password: "{{ vaulted_bind_password }}"
base_dn: 'OU=users,DC=yourdomain,DC=com'
group_search: 'DC=yourdomain,DC=com'
admins: 'CN=toweradmins,OU=Groups,DC=yourdomain,DC=com'
users: 'CN=towerusers,OU=Groups,DC=yourdomain,DC=com'
```

`install_certificate: true` - Do you want to install a custom SSL
certificate? If so put your vaulted cert and keys here:

```
/vars/vault/certificates/tower.cert
/vars/vault/certificates/tower.key
```

`configure_kerberos: true` - Do you want to enable Kerberos?

```
# Kerberos configuration
default_realm: "yourdomain.com"

realms:
  yourdomain.com:
    kdc1: dc1.yourdomain.com
    kdc2: dc2.yourdomain.com
```
`upgrade_ansible: true` - Do you want to upgrade ansible yum package?

`create_backup: true` - Do you want to create backup and upload it to azure storage?
```
# Backup configuration
resource_group: azure_resource_group_name
storage_account: azure_storage_account_name
```

`-e backup_name=/tmp/tower-backup.tar.gz` - Do you want to restore from backup?
```
ansible-playbook ansible-tower.yml -i "ip,"-e backup_name=/tmp/tower-backup-2018-06-04.tar.gz
File will be uploaded to insance and tower configuration will be restored from this file
```

It's recommended you vault the following required vars:
```
vaulted_admin_password: 'password'
vaulted_rabbitmq_password: 'password'
vaulted_postgresql_password: 'password'
vaulted_bind_dn: 'cn=ldapbinduser,ou=users,dc=domain,dc=com'
vaulted_bind_password: 'ldapbindpassword'
```

Example Playbook
----------------

Example playbook with vaulted variables.

    - hosts: all
      become: yes
      vars_files:
        - vars/vault/vault.yml
        - vars/vars.yml
      roles:
        - { role: tower }

License
-------
MIT

Author Information
------------------
This project is developed and maintained by:
https://github.com/infectsoldier Maksym Postument
and
https://github.com/asagage Asa Gage
