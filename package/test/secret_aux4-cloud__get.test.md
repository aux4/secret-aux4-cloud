# secret aux4-cloud get

## resolving requested fields

```beforeAll
aux4 mock start --port 8871
aux4 mock stub --port 8871 --method GET --path /v1/dev/secrets/customer_db --status 200 --body '{"name":"customer_db","fields":{"username":"sa","password":"abc123"}}'
for i in $(seq 1 50); do curl -s -o /dev/null "http://localhost:8871/api/v1/dev/secrets/customer_db" && break; sleep 0.2; done
```

```afterAll
aux4 mock stop --port 8871
```

### should emit a flat JSON object of only the requested fields

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8871/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud get --ref customer_db --fields "username,password"
```

```expect:json
{
  "username": "sa",
  "password": "abc123"
}
```

### should project only a single requested field

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8871/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud get --ref customer_db --fields "username"
```

```expect:json
{
  "username": "sa"
}
```

### should send the bearer token from CLOUD_SYNC_TOKEN

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8871/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud get --ref customer_db --fields "username" && aux4 mock verify --port 8871 --method GET --path /v1/dev/secrets/customer_db --header Authorization="Bearer test-token"
```

```expect:partial
verify ok: **
```
