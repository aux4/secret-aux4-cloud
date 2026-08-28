#### Description

Get specific fields from an aux4.cloud secret as a flat JSON object. This is the command the aux4 core invokes when resolving `secret://aux4-cloud/<item>/<field>` URIs, so its output must be a single JSON object of the requested fields and nothing else.

The provider issues `GET ${AUX4_CLOUD_API_URL}/v1/${AUX4_CLOUD_SCOPE}/secrets/<ref>` with an `Authorization: Bearer <token>` header. The API returns the full item; this command projects out only the fields named in `--fields`.

- **Multiple fields** — returns a JSON object with each requested field as a key.
- **Field projection** — only the fields listed in `--fields` are included in the output.
- **OTP** — aux4.cloud has no separate one-time-password concept. The `--otp` flag exists for provider compatibility and has no effect; an `otp` field, if stored on the item, resolves like any other field.

Authentication and connection are resolved from the environment: `CLOUD_SYNC_TOKEN` (machine API key on aux4.cloud VMs) or the current SSO session, `AUX4_CLOUD_API_URL`, and `AUX4_CLOUD_SCOPE`.

#### Usage

```bash
aux4 secret aux4-cloud get --ref <item> --fields <field1,field2,...> [--otp <true|false>]
```

--ref      The secret item name, e.g. `customer_db` (required)
--fields   Comma-separated field names to retrieve (required)
--otp      Accepted for compatibility, ignored by this provider (default: false)

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
