# One-time Vault Kubernetes auth setup

Run these commands with a Vault administrator token after the Kubernetes auth
method is enabled at `auth/kubernetes`.

```bash
vault policy write cagea1-vso - <<'EOF'
path "kv/data/cagea1-com-prod-secret" {
  capabilities = ["read"]
}
EOF

vault write auth/kubernetes/role/cagea1-vso \
  bound_service_account_names=cagea1-vault-auth \
  bound_service_account_namespaces=cagea1-prod,cert-manager \
  policies=cagea1-vso \
  ttl=1h
```

The policy allows only read access to `kv/data/cagea1-com-prod-secret`.
