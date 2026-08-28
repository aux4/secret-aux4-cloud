#### Description

Remove a secret from the current aux4.cloud scope. The provider issues `DELETE ${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets/<ref>` with an `Authorization: Bearer <token>` header. This deletes the entire item and all of its fields.

#### Usage

```bash
aux4 secret aux4-cloud remove --ref <item>
```

--ref   The secret item name, e.g. `customer_db` (required)

#### Example

```bash
aux4 secret aux4-cloud remove --ref customer_db
```

```text
secret://aux4-cloud/customer_db removed
```
