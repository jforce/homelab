# Homelab - SNO OCP

# Build
To run the build playbook to install SNO:
```bash
ansible-playbook homelab_sno_build.yaml --vault-password-file .vault_pass.txt
```

# Power
To power the server run the below playbook specifying `power` to be `on` or `off`:
```bash
ansible-playbook power.yaml --vault-password-file .vault_pass.txt --extra-vars power=on
ansible-playbook power.yaml --vault-password-file .vault_pass.txt --extra-vars power=off
```
