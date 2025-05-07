# Setup EC2 Collection and Authentication

## Install boto3

```
pip install boto3
```

## Install AWS Collection

```
ansible-galaxy collection install amazon.aws
```

## Setup Vault 

1. Create a password for vault

```
echo 'mypassword123' > vault.pass
(or)
openssl rand -base64 2048 > vault.pass
```

2. Add your AWS credentials using the below vault command

```
ansible-vault create group_vars/all/pass.yml --vault-password-file vault.pass
```

3. set vault password file in aws secret manager

   ```
 aws secretsmanager create-secret \
  --name ansible/vault-password \
  --secret-string 'your-strong-vault-password'
  ```

4. bash script to get vault password

   ```
#!/bin/bash
aws secretsmanager get-secret-value \
  --secret-id ansible/vault-password \
  --query SecretString \
  --output text
  ```

5. Run playbook with vault password which is in aws secrets manager

    ```
chmod +x get-vault-pass.sh

ansible-playbook playbook.yml --vault-password-file ./get-vault-pass.sh

  ```
   




