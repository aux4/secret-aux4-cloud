# aux4/secret-aux4-cloud

aux4.cloud secret provider for [aux4/secret](https://hub.aux4.io/r/public/packages/aux4/secret). It resolves `secret://aux4-cloud/<item>/<field>` references by talking to the aux4.cloud control-plane API over HTTPS. Secrets are fetched at execution time so credentials never appear in configuration files, and the provider never touches AWS directly.

## Installation

```bash
aux4 aux4 pkger install aux4/secret-aux4-cloud
```

## Prerequisites

- `jq` installed (auto-installed via the package manager)
- `curl` installed (auto-installed via the package manager)

## Configuration

The provider reads all connection settings from environment variables — nothing is stored in config files or passed as command flags:

| Variable | Description |
|----------|-------------|
| `AUX4_CLOUD_API_URL` | Base URL of the aux4.cloud API (e.g. `https://dev.api.aux4.cloud`) |
| `AUX4_CLOUD_SCOPE` | The scope whose secrets are being accessed |
| `CLOUD_SYNC_TOKEN` | Machine API key, injected automatically when running on an aux4.cloud VM |

Authentication is resolved automatically:

- When `CLOUD_SYNC_TOKEN` is set (the case on an aux4.cloud VM), that machine API key is used.
- Otherwise the current SSO session token is used via `aux4 aux4 token` (the laptop case).

Every request is sent with an `Authorization: Bearer <token>` header. Requests target `${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets`.

## Reference Format

The `--ref` parameter is the item name:

```text
customer_db
```

When used inline by consuming packages, the full `secret://` URI includes the provider prefix:

```text
secret://aux4-cloud/<item>/<field>
```

For example, a variable default of `secret://aux4-cloud/customer_db/password` resolves to the `password` field of the `customer_db` item.

## Commands

### Get

Get specific fields from a secret as a flat JSON object. This is the command the aux4 core invokes when resolving `secret://aux4-cloud/...` URIs.

```bash
aux4 secret aux4-cloud get --ref customer_db --fields "username,password"
```

```json
{
  "username": "sa",
  "password": "abc123"
}
```

Only the requested fields are returned. aux4.cloud has no separate one-time-password concept — an `otp` field, if stored on the item, resolves like any other field.

### Set

Update a single field of an existing secret. The change merges into the item's existing fields.

```bash
aux4 secret aux4-cloud set --ref customer_db --field password --value newpass123
```

```text
secret://aux4-cloud/customer_db updated
```

### Create

Create a new secret. Fields are provided as comma-separated `key=value` pairs. The `--vault` and `--category` options are accepted for compatibility with other providers but are ignored by aux4.cloud.

```bash
aux4 secret aux4-cloud create --item customer_db --fields "username=sa,password=abc123"
```

```text
secret://aux4-cloud/customer_db
```

### List

List all secrets in the scope as `secret://` references, one per field.

```bash
aux4 secret aux4-cloud list
```

```text
secret://aux4-cloud/customer_db/username
secret://aux4-cloud/customer_db/password
```

### Remove

Delete a secret from the scope.

```bash
aux4 secret aux4-cloud remove --ref customer_db
```

```text
secret://aux4-cloud/customer_db removed
```

## Environment Variables

- `AUX4_CLOUD_API_URL` — base URL of the aux4.cloud API.
- `AUX4_CLOUD_SCOPE` — the scope whose secrets are accessed.
- `CLOUD_SYNC_TOKEN` — machine API key used automatically when present (aux4.cloud VMs).
