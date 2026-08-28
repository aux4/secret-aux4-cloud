#### Description

Create a new aux4.cloud secret. Fields are provided as comma-separated `key=value` pairs. The provider parses the pairs with `jq` into a `{ "fields": { ... } }` body and issues `PUT ${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets/<item>` with an `Authorization: Bearer <token>` header.

Values may themselves contain `=`; only the first `=` in each pair separates the key from the value. Keys and values are passed to `jq` as arguments, so they are handled safely.

The `--vault` and `--category` options are accepted for compatibility with other secret providers but are ignored by aux4.cloud.

#### Usage

```bash
aux4 secret aux4-cloud create --item <item> --fields <k1=v1,k2=v2,...> [--vault <vault>] [--category <category>]
```

--item      The item name (required)
--fields    Comma-separated field assignments, e.g. `username=sa,password=abc123` (required, hidden input)
--vault     Accepted for compatibility, ignored by this provider (default: empty)
--category  Accepted for compatibility, ignored by this provider (default: Login)

#### Example

```bash
aux4 secret aux4-cloud create --item customer_db --fields "username=sa,password=abc123"
```

```text
secret://aux4-cloud/customer_db
```
