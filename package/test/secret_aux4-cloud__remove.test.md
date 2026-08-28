# secret aux4-cloud remove

## removing a secret

```beforeAll
aux4 mock start --port 8875
aux4 mock stub --port 8875 --method DELETE --path /v1/dev/secrets/customer_db --status 200 --body '{"ok":true}'
for i in $(seq 1 50); do curl -s -o /dev/null -X DELETE "http://localhost:8875/api/v1/dev/secrets/customer_db" && break; sleep 0.2; done
```

```afterAll
aux4 mock stop --port 8875
```

### should confirm the removal

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8875/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud remove --ref customer_db
```

```expect
secret://aux4-cloud/customer_db removed
```

### should issue a DELETE request for the item

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8875/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud remove --ref customer_db && aux4 mock verify --port 8875 --method DELETE --path /v1/dev/secrets/customer_db
```

```expect:partial
verify ok: **
```
