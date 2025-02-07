# Create Akeyless API key for Gateway
Reference
(https://docs.akeyless.io/docs/gateway-chart#authentication)


## Configure Akeyless CLI with SaaS
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

## Create auth_method using API Key 

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

## Create Access Role and associate the role with API key 
```
akeyless create-role --profile default -n /devops/devops-api-role
akeyless list-roles --profile default --filter '/devops/devops-api-role'
akeyless assoc-role-am -r  /devops/devops-api-role -a /devops/devops-api-key
```

## Set rules for Access Role 
```
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type item-rule -c read -c create -c update -c delete -c delete -c list
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type role-rule -c read -c create -c update -c delete -c delete -c list
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type target-rule -c read -c create -c update -c delete -c delete -c list
akeyless set-role-rule -r /devops/devops-api-role -p "/devops/*" --rule-type auth-method-rule -c read -c create -c update -c delete -c delete -c list
```

## Store the access key for the gateway with defined key
```
kubectl create secret generic access-key --namespace waynez \
  --from-literal=gateway-access-key=JSomethingSpecial=
```
