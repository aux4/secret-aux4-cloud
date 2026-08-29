#### Description

Create a new aux4.cloud secret. Fields are provided as dot-notation flags — one `--fields.<name> <value>` per field — which aux4 collects into a single object. The provider issues `PUT ${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets/<item>` with the fields as the JSON body and an `Authorization: Bearer <token>` header.

Each value is shell-escaped by aux4, so values may safely contain quotes, spaces, or `=`.

Because a `PUT` merges into any existing item, `create` and `set` share the same endpoint; `create` is simply the convenient way to write several fields at once.

The `--vault` and `--category` options are accepted for compatibility with other secret providers but are ignored by aux4.cloud.

#### Usage

```bash
aux4 secret aux4-cloud create --item <item> --fields.<name> <value> [--fields.<name2> <value2> ...] [--vault <vault>] [--category <category>]
```

--item             The item name (required)
--fields.<name>    A field value; repeat the flag once per field (required, hidden input)
--vault            Accepted for compatibility, ignored by this provider (default: empty)
--category         Accepted for compatibility, ignored by this provider (default: Login)

#### Example

```bash
aux4 secret aux4-cloud create --item customer_db --fields.username sa --fields.password abc123
```

```text
secret://aux4-cloud/customer_db
```
