# Create cluster issuer with nginx ingress. 

Note: private key secrect reference will be created at runtime, no need to create earlier.
And the secret will be rotated automatically by Cert Manager.

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