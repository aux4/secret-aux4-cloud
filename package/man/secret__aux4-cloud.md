#### Description

Routes to the aux4.cloud secret provider commands. This provider plugs into `aux4/secret` and resolves `secret://aux4-cloud/<item>/<field>` references by calling the aux4.cloud control-plane API over HTTPS. It never touches AWS directly.

Available subcommands:

- **get** — retrieve secret fields as a flat JSON object (the `secret://` resolution path)
- **set** — update a single field of an existing secret
- **create** — create a new secret from `key=value` pairs
- **list** — list all secrets in the scope as `secret://` references
- **remove** — delete a secret

Connection settings come from the environment: `AUX4_CLOUD_API_URL`, `AUX4_CLOUD_SCOPE`, and (on aux4.cloud VMs) `CLOUD_SYNC_TOKEN`.

#### Usage

```bash
aux4 secret aux4-cloud <command>
```

#### Example

```bash
aux4 secret aux4-cloud get --ref customer_db --fields "username,password"
```

```json
{
  "username": "sa",
  "password": "abc123"
}
```
