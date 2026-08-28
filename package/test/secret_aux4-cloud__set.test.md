# secret aux4-cloud set

## updating a field

```beforeAll
aux4 mock start --port 8872
aux4 mock stub --port 8872 --method PUT --path /v1/dev/secrets/customer_db --status 200 --body '{"ok":true}'
for i in $(seq 1 50); do curl -s -o /dev/null -X PUT "http://localhost:8872/api/v1/dev/secrets/customer_db" && break; sleep 0.2; done
```

```afterAll
aux4 mock stop --port 8872
```

### should confirm the update

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8872/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud set --ref customer_db --field password --value newpass123
```

```expect
secret://aux4-cloud/customer_db updated
```

### should PUT a merge body with the field and value

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8872/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud set --ref customer_db --field password --value newpass123 && aux4 mock verify --port 8872 --method PUT --path /v1/dev/secrets/customer_db --body-contains '"password":"newpass123"'
```

```expect:partial
verify ok: **
```
