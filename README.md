# LDAP Authentication

Ldap configuration to authenticate user using django auth ldap.

## Overview
Ldap configuration to authenticate user through active directory using django auth ldap.

## Requirements
* django_auth_ldap
* Python_ldap > 3.0


## Configurations

1. Add authentication backend in settings.py

    AUTHENTICATION_BACKENDS = [
        'django_auth_ldap.backend.LDAPBackend',
        'django.contrib.auth.backends.ModelBackend',
    ]

2. Bypass SSL
     
    ldap.set_option(ldap.OPT_X_TLS_REQUIRE_CERT, ldap.OPT_X_TLS_NEVER)

3. Individual User authentication

    AUTH_LDAP_SERVER_URI = "ldaps://yourDomain:636"
    
    AUTH_LDAP_BASE_DN = "DC=ReplaceWithYourValue,DC=corp,DC=com"  e.g. ReplaceWithYourValue=google
    
    AUTH_LDAP_BIND_DN = "CN=YourCNValue,OU=Users,OU=Service Accounts,DC=ReplaceWithYourValue,DC=corp,DC=com"
    
    AUTH_LDAP_BIND_PASSWORD = "Enter Password Here"
    
    AUTH_LDAP_USER_SEARCH = LDAPSearch(AUTH_LDAP_BASE_DN, ldap.SCOPE_SUBTREE, "(sAMAccountName=%(user)s)")

4. Group based user authentication

    AUTH_LDAP_GROUP_SEARCH = LDAPSearch("OU=Users,OU=Service Accounts,DC=ReplaceWithYourValue,DC=corp,DC=com",
                                    ldap.SCOPE_SUBTREE, "(&(objectClass=user)(sAMAccountName=%(user)s))"
                                    )

    AUTH_LDAP_GROUP_TYPE = GroupOfNamesType()
    
    AUTH_LDAP_REQUIRE_GROUP = "CN=GroupNameHere,OU=GroupNameHere,DC=ReplaceWithYourValue,DC=corp,DC=com"
    
    AUTH_LDAP_USER_ATTR_MAP = {
        "username": "sAMAccountName",
        "first_name": "name",
        "last_name": "displayname",
        "email": "mail",
    }

    AUTH_LDAP_FIND_GROUP_PERMS = True
    
5. Logging


    import logging
    
    logger = logging.getLogger('django_auth_ldap')
    
    logger.addHandler(logging.StreamHandler())
    
    logger.setLevel(logging.DEBUG)
