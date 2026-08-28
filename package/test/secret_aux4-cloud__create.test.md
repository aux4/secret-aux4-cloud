# secret aux4-cloud create

## creating a secret

```beforeAll
aux4 mock start --port 8873
aux4 mock stub --port 8873 --method PUT --path /v1/dev/secrets/customer_db --status 200 --body '{"ok":true}'
for i in $(seq 1 50); do curl -s -o /dev/null -X PUT "http://localhost:8873/api/v1/dev/secrets/customer_db" && break; sleep 0.2; done
```

```afterAll
aux4 mock stop --port 8873
```

### should print the secret reference

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8873/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud create --item customer_db --fields "username=sa,password=abc123"
```

```expect
secret://aux4-cloud/customer_db
```

### should PUT a fields body parsed from the key=value pairs

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8873/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud create --item customer_db --fields "username=sa,password=abc123" && aux4 mock verify --port 8873 --method PUT --path /v1/dev/secrets/customer_db --body-contains '"username":"sa"'
```

```expect:partial
verify ok: **
```

### should keep equals signs inside a field value

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8873/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud create --item customer_db --fields "token=ab=cd" && aux4 mock verify --port 8873 --method PUT --path /v1/dev/secrets/customer_db --body-contains '"token":"ab=cd"'
```

```expect:partial
verify ok: **
```
