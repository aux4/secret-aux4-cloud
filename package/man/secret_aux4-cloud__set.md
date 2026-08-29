#### Description

Update a single field of an existing aux4.cloud secret. The provider builds the request body from the field name and value and issues `PUT ${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets/<ref>` with an `Authorization: Bearer <token>` header. The named field is merged into the item's existing fields; other fields are left untouched.

The value is shell-escaped by aux4, so values containing quotes, spaces, or other special characters are handled safely.

#### Usage

```bash
aux4 secret aux4-cloud set --ref <item> --field <name> --value <value>
```

--ref     The secret item name, e.g. `customer_db` (required)
--field   The field name to update (required)
--value   The new value (required, hidden input)

#### Example

```bash
aux4 secret aux4-cloud set --ref customer_db --field password --value newpass123
```

```text
secret://aux4-cloud/customer_db updated
```
