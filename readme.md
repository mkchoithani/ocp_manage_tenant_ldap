# ocp_manage_tenant_ldap_users

Creates and deletes groups in an LDAP server which follow the naming convention for OCP Tenant spaces.

In addition, it adds and removes existing users into the above groups if required.

## Creating Tenant Groups example

```yaml
- host: localhost
  name: creating tenant groups
  vars:
    ldap_server_uri: 'ldap://<<ip>>:<<port>>'
    group_dn: 'OU=Groups,OU=<<sub_OU_name>>,OU=<<main_OU_name>>,DC=<<domain>>,DC=com'
    local_admin_dn: '<<admin_name>>'
    local_admin_pwd: '<<admin_pwd>>'
    tenant_name: 'Tenant_A'
    projects:
        - name: 'DEV'
        - name: 'TEST'
        - name: 'DEMO'
  include_role: 
    name: ocp_manage_tenant_ldap_users
    tasks_from: create_groups
```

for example:

- group_dn: **OU=Groups,OU=Instance01,OU=OCP_Instances,DC=myldap,DC=com**
- local_admin_dn: **CN=AdminUser,CN=Users,DC=myldap,DC=com**
- local_admin_pwd: **XSXSidje**

## Deleting Tenant Groups example

```yaml
- host: localhost
  name: deleting tenant groups
  vars:
    ldap_server_uri: 'ldap://<<ip>>:<<port>>'
    group_dn: 'OU=Groups,OU=<<sub_OU_name>>,OU=<<main_OU_name>>,DC=<<domain>>,DC=com'
    local_admin_dn: '<<admin_name>>'
    local_admin_pwd: '<<admin_pwd>>'
    tenant_name: 'Tenant_A'
    projects:
        - name: 'DEV'
        - name: 'TEST'
        - name: 'DEMO'
  include_role: 
    name: ocp_manage_tenant_ldap_users
    tasks_from: delete_groups
```

## Adding users to groups example


```yaml
   - host: localhost
     name: adding users to tenant groups
     vars:
       ldap_server_uri: 'ldap://<<ip>>:<<port>>'
       group_dn: 'OU=Groups,OU=<<sub_OU_name>>,OU=<<main_OU_name>>,DC=<<domain>>,DC=com'
       local_admin_dn: '<<admin_name>>'
       local_admin_pwd: '<<admin_pwd>>'
       tenant_name: 'Tenant_A'
       projects: 
         - name: 'DEV'
           admins:
             - devAdmin@acme.com
           users:
             - dev1@acme.com
             - dev2@acme.com
         - name: 'TEST'
           admins:
             - testAdmin@acme.com
           users:
               - tester1@acme.com
               - tester2@acme.com
         - name: 'DEMO'
             admins:
               - demoAdmin@acme.com
             users:
               - demo1@acme.com
               - demo2@acme.com
     include_role: 
       name: ocp_manage_tenant_ldap_users
       tasks_from: add_users_to_groups
   ```
   
## Removing users from groups example

```yaml
- host: localhost
  name: removing users from tenant groups
  vars:
    ldap_server_uri: 'ldap://<<ip>>:<<port>>'
    group_dn: 'OU=Groups,OU=<<sub_OU_name>>,OU=<<main_OU_name>>,DC=<<domain>>,DC=com'
    local_admin_dn: '<<admin_name>>'
    local_admin_pwd: '<<admin_pwd>>'
    tenant_name: 'Tenant_A'
    projects: 
      - name: 'DEV'
        admins:
          - devAdmin@acme.com
        users:
          - dev1@acme.com
          - dev2@acme.com
      - name: 'TEST'
          admins:
            - testAdmin@acme.com
          users:
            - tester1@acme.com
            - tester2@acme.com
      - name: 'DEMO'
          admins:
            - demoAdmin@acme.com
          users:
            - demo1@acme.com
            - demo2@acme.com
  include_role: 
    name: ocp_manage_tenant_ldap_users
    tasks_from: remove_users_from_groups
```