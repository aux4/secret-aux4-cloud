#### Description

List all aux4.cloud secrets in the current scope as `secret://` references, one line per field. The provider issues `GET ${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets` with an `Authorization: Bearer <token>` header and expands each item's fields into `secret://aux4-cloud/<item>/<field>` references. The output is plain text intended for humans.

#### Usage

```bash
aux4 secret aux4-cloud list
```

#### Example

```bash
aux4 secret aux4-cloud list
```

```text
secret://aux4-cloud/customer_db/username
secret://aux4-cloud/customer_db/password
```
