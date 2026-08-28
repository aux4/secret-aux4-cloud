# secret aux4-cloud list

## listing secrets

```beforeAll
aux4 mock start --port 8874
aux4 mock stub --port 8874 --method GET --path /v1/dev/secrets --status 200 --body '{"secrets":[{"name":"customer_db","fields":["username","password"]}]}'
for i in $(seq 1 50); do curl -s -o /dev/null "http://localhost:8874/api/v1/dev/secrets" && break; sleep 0.2; done
```

```afterAll
aux4 mock stop --port 8874
```

### should print one secret:// reference per field

```execute
CLOUD_SYNC_TOKEN=test-token AUX4_CLOUD_API_URL=http://localhost:8874/api AUX4_CLOUD_SCOPE=dev aux4 secret aux4-cloud list
```

```expect
secret://aux4-cloud/customer_db/username
secret://aux4-cloud/customer_db/password
```
