# Release Notes

Rewritten in pure aux4 DSL. The provider no longer shells out to `curl` and `jq`; it now composes the `aux4/curl` and `aux4/json` packages instead, which pkger installs automatically as dependencies. The `jq` and `curl` system prerequisites are gone.

## Changed

- **`create`** now takes fields as dot-notation flags — `--fields.username sa --fields.password abc123` — instead of a comma-separated `key=value` string. Values are shell-escaped by aux4, so quotes, spaces, and `=` are safe.
- Request bodies are built with aux4's native `object()` / `value()` functions and parsed with `aux4/json`, replacing the previous `jq` pipelines.
- HTTP calls go through `aux4 curl request` rather than the system `curl` binary.

## Unchanged

- `secret://aux4-cloud/<item>/<field>` resolution, the `get`/`set`/`list`/`remove` interfaces, environment-variable configuration (`AUX4_CLOUD_API_URL`, `AUX4_CLOUD_SCOPE`, `CLOUD_SYNC_TOKEN`), and Bearer-token authentication (machine key, falling back to the logged-in aux4 token) all behave exactly as before.
