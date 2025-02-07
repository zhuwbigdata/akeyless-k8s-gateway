# Okta on SAML 

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


