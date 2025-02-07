# Okta on SAML 

## Create SAML auth method 
```
Add create CMD here ...

akeyless auth-method list --filter /devops/okta_saml_auth_using_subclaim
```

## Create Access Roles and associate with SAML auth method
```
Add create CMD here
akeyless list-roles --filter /devops/devops_TL
akeyless list-roles --filter /devops/devops_ENGR
```

## Set rules for Access Roles
- RBAC on /devops/devops_TL
  - Items on /devops/*: list,read, create, update, delete
  - Access Roles on /devops/*: list,read, create, update, delete
  - Auth Methods on /devops/*: list,read, create, update, delete
- RBAC on /devops/devops_ENGR: 
  - Items on /devops/*: list, read
```
Add CMD here
```


## Create Okta dev instance (personal)
```
https://dev-{your_own_id}-admin.okta.com
```

## Create OKTA SAML Application
OKTA > Applications > Create App Integration as follows:

SAML settings
- Sign-on Method: SAML 2.0
- App Name: akeyless_saml_access
- SSO URL: https://auth.akeyless.io/saml/acs
- Recepient URL: as SSO
- Destination URL: as SSO
- Audience URI: https://auth.akeyless.io/saml/metadata
- Attribute Statements:
    - email: user.email
    - user: user.login
- Group Attribute Statements:
- groups, Matches regex=.* (doesn't work for me)
- groups, basic, Equals:devops
- App type: This is an internal app that we have created

User and groups
- groups: devops
- users: user1 (manager), user2 (engineer)


