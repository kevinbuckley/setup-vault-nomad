currently the hackiest, hack that ever hacked.  integrating nomad & vault 

```bash
# run consul server

consul agent -dev

# run vault server
vault server -dev

# setup vault policies and roles
vault policy write nomad-server nomad-server-policy.hcl    # policy for what nomad can do with vault
vault write /auth/token/roles/nomad-cluster @nomad-cluster-role.json # creates the role for nomad to use

# create a token for nomad to use and start nomad agent
sudo VAULT_TOKEN=$(vault token create -policy nomad-server -period 72h -orphan | grep -w "token" |  sed 's/.* //') nomad agent -config nomad-server.hcl -config nomad-client.hcl

```