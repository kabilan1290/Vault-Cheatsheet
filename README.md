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

# Dynamic Secret

vault write aws/config/root \
    access_key=AKIAI4SGLQPBX6CSENIQ \
    secret_key=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 \
    region=us-east-1

Success! Data written to: aws/config/root                  - Storing the credentials in AWS secret engine.

 vault write aws/roles/my-role \
        credential_type=iam_user \
        policy_document=-<<EOF
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "Stmt1426528957000",
      "Effect": "Allow",
      "Action": [
        "ec2:*"
      ],
      "Resource": [
        "*"
      ]
    }
  ]
}
EOF
Success! Data written to: aws/roles/my-role                 - Creating a AWS role in Vault.

vault read aws/creds/my-role - To generate Secret.

vault lease revoke aws/creds/my-role/0bce0782-32aa-25ec-f61d-c026ff22106 - Revoking AWS key using lease id,By default it will be destroyed after 768 hours.

vault token create - To create a Token.

vault login xxxxxxx - To login with authentication token.

vault token revoke xxxx - To revoke the authentication token.

vault auth enable github - To enable github authentication.

vault write auth/github/config organization=xxx - To enable authentication on organization.

vault auth list -To list all authentication methods.

vault login -method=github - To attempt login using github.

vault token revoke -mode path auth/github - Revoke all the tokens geenrated using github.

vault auth disable github - To disable github authentication.

vault policy read default -To View deafult policy.

vault policy write my-policy - << EOF
path "secret/data/*" {
  capabilities = ["create", "update"]   - To write a Policy
}
EOF

vault policy list - To list the policies

vault auth enable approle - To enable auth role >> associate policies with auth methods

mkdir -p vault/data
vault server -config=config.hcl - Deploy server with configuration.

curl \
    --request POST \
    --data '{"secret_shares": 1, "secret_threshold": 1}' \ - To access Vault secret using REST Api.
    http://127.0.0.1:8200/v1/sys/init | jq 
    
curl http://127.0.0.1:8200/v1/sys/init - Rest api to view the intialization status.

To activate the web interface we need to add the below code in config.hcl

ui = true

listener "tcp" {
  address = "10.0.1.35:8200"
}

Starting the valut server with the config file will lead you to Web Application of vault.
