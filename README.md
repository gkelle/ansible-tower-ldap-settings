ansible-tower-ldap-settings
=========

This role will enable you to update the Ansible Tower LDAP configuration using an ansible dictionary.

Requirements
------------

You will need a username/password combination or a token of a user that is able to manage LDAP settings in Ansible Tower.

Role Variables
--------------

tower_host: The IP or fqdn of the Ansible Tower UI. (Default: Null) 
tower_user: The user who will connect to Ansible Tower to perform the LDAP update. (Default: Null)  
tower_password: The password of the user who will connect to Ansible Tower to perform the LDAP update. (Default: Null)  
tower_token: A token of a user who will connect to Ansible Tower to perform the LDAP update. (Default: Null)  

Note: If you are defining a tower_token, there is no need to define tower_user or tower_password.  


tower_uri_force_basic_auth: Force sending of basic authentication header upon initial request. (Default: yes)  
tower_uri_method_detect: Determine automatically whether the API method should be PUT or PATCH. Only use PATCH if no bind password is defined. (Default: True)  
tower_uri_method: API method to use. (Default: PUT)  
tower_uri_validate_certs: Validate that https certificate is valid (Default: no)  

Note: If you set tower_ldap_uri_method_detect to False, you can specify whatever tower_ldap_uri_method you want and have it used as-is.  

secure_tower_ldap_settings: A dictionary of secure fields to set.  It is recommended to put these in a vault, for example the setting for AUTH_LDAP_BIND_PASSWORD.  
tower_ldap_settings: A dictionary of LDAP fields to set.  
default_tower_ldap_settings: A dictionary of default LDAP fields.  

For a reference of what fields are available, please see defaults/main.yml for more information.  
Also: https://docs.ansible.com/ansible-tower/latest/html/administration/ldap_auth.html


Example Playbook
----------------

    - hosts: localhost
      roles:
         - role: ansible-tower-ldap-settings
           vars:
             tower_host: 10.10.10.100
             tower_ldap_settings:
               AUTH_LDAP_SERVER_URI: ldaps://ldap-server
               AUTH_LDAP_BIND_DN: uid=my_tower_bind,cn=users,cn=accounts,dc=example,dc=com
               AUTH_LDAP_USER_DN_TEMPLATE: uid=%(user)s,cn=users,cn=accounts,dc=example,dc=com
               AUTH_LDAP_USER_ATTR_MAP:
                   first_name: givenName
                   last_name: sn
                   email: mail
               AUTH_LDAP_GROUP_SEARCH:
                   - cn=groups,cn=accounts,dc=example,dc=com
                   - SCOPE_SUBTREE
                   - (objectClass=groupofnames)
               AUTH_LDAP_ORGANIZATION_MAP:
                   LDAP Organization:
                       admins: cn=my_org_admins,cn=groups,cn=accounts,dc=example,dc=com
                       remove_admins: false
                       users:
                           - cn=my_org_users,cn=groups,cn=accounts,dc=example,dc=com
                       remove_users: false
               AUTH_LDAP_TEAM_MAP:
                   LDAP Engineering:
                       organization: LDAP Organization
                       users: cn=my_org_engineering_team,cn=groups,cn=accounts,dc=example,dc=com
                       remove: true

License
-------

BSD
