# akeyless-k8s-gateway

## Pre-requisite

### Nginx Ingress and Cert-Manager Deployment
Reference
```
https://docs.nginx.com/nginx-ingress-controller/overview/design/
https://gist.github.com/devorbitus/a292aa1bed559c9b87053aa0fe21d094
```
This should be deployed before installing the Akeyless gateway.
Once deployed, identify the external IP address for ingress-nginx-controller.

### DNS configuration 
GCP - Cloud DNS

AWS - Route 53

Azure -  Azure DNS

Notes: if your hosted DNS registrar is not your domain controller, you need to add an NS
record in your domain controller.

### Create cluster issuer with nginx ingress. 

Note: private key secrect reference will be created at runtime, no need to create earlier.
```
cat <<EOF | kubectl apply -f -
---
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
    annotations: {}
    name: wzhu-letsencrypt-prod
spec:
    acme:
      email: wayne.z@akeyless.io
      preferredChain: ""
      privateKeySecretRef:
        name: letsencrypt-prod
      server: https://acme-v02.api.letsencrypt.org/directory
      solvers:
      - http01:
          ingress:
            class: nginx
---
EOF
```

### Utilities - gcloud, terraform, helm, Akeyless CLI
Install GCP cloud SDK using brew
```
brew install --cask google-cloud-sdk
gcloud components install kubectl
gcloud components install gke-gcloud-auth-plugin
```


Install Terraform and Helm
```
brew tap hashicorp/tap
brew install hashicorp/tap/terraform
brew install helm
```
Install Akeyless CLI
```
brew install akeylesslabs/tap/akeyless
```

Configure Akeyless using profile with admin priviledges
```
akeyless configure --profile default 
```

List configured profiles 
```
ls -al ~/.akeyless/profiles/
cat ~/.akeyless/profiles/default.toml
```
Delete a Profile (Optional)
```
akeyless unconfigure --profile default
```

### Create auth_method for Akeyless Gateway
Reference
```
https://docs.akeyless.io/docs/gateway-chart#authentication
```

#### API key
```
akeyless auth-method create api-key --profile default --name /devops/devops-api-key --json true | tee 
devops-api-key.json
{
  "name": "/devops/devops-api-key",
  "access_id": "p-xyz",
  "access_key": "JSomethingSpecial="
}
akeyless auth-method list --filter '/devops/devops-api-key'
```

Create Access Role and its rules and associate the role with Auth Method
```
akeyless create-role --profile default -n /devops/devops-api-role
akeyless list-roles --profile default --filter '/devops/devops-api-role'
akeyless assoc-role-am -r  /devops/devops-api-role -a /devops/devops-api-key

akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type item-rule -c read -c create -c update -c delete -c delete -c list
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type role-rule -c read -c create -c update -c delete -c delete -c list
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type target-rule -c read -c create -c update -c delete -c delete -c list
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type auth-method-rule -c read -c create -c update -c delete -c delete -c list
```
Store the access key for the gateway with defined key
```
kubectl create secret generic access-key --namespace waynez \
  --from-literal=gateway-access-key=JSomethingSpecial=
```
#### Cloud Native with GCP 


### User Provision

#### Okta/SAML 
Okta dev instance (personal)
```
https://dev-{your_own_id}-admin.okta.com
```

Create OKTA SAML Application

    - OKTA > Applications > Create App Integration as follows:

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


