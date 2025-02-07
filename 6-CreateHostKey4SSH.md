# Create host key for SRA ssh access

## Create SRA signing key using Encryption Key /DFC
```
akeyless create-dfc-key --name /devops/sra-signing-key --alg RSA2048

```

## Configure SSH CA ssh-rsa
```
akeyless get-rsa-public --name  /devops/sra-signing-key
```

## Create SSH Certificate Issuer
Under /devops/ssh_host_key

```
Add CMDs here
akeyless update-ssh-cert-issuer \
  --name my-ssh-cert-issuer \
  --secure-access-enable true \
  --secure-access-bastion-api <SRA-Bastion API Endpoint> \
  --secure-access-bastion-ssh <SRA-Bastion IP and Port> \
  --secure-access-ssh-creds-user <SSH username> \
  --host-provider explicit \
  --secure-access-host <remote host>
```

