# Vault-Cheatsheet

To Download Vault - https://www.vaultproject.io/downloads

# Running a Dev Server
vault server -dev - To run dev server,useful for understanding and playing around with vault.

Then we need to set the environment variables

export VAULT_ADDR='http://127.0.0.1:8200'

export VAULT_TOKEN="xxxxxxxxxxxxxxxxxxx"

Vault status - To check running

# Writing a secret

vault kv put secret/hello foo=world - This write the foo/world key pair to secret/hello path.

vault kv get secret/hello - The written secret can be retrived using the following command.

vault kv get -field=excited secret/hello - If we have number of secrets added,we can use the field flag to mention the key and retrieve the value associated with it.

vault kv delete secret/hello - Deleting the secret.

vault secrets list - To list the secret engines

vault secrets enable -path=kv kv - To enable secret engine path named kv.

```
Now we can add secrets to the new secret engine path
```

vault kv list kv/ - To list the key/value pairs.

vault secrets disable kv/ - To disable the secret path.
