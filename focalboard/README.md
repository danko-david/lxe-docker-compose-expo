# Focalboard

## Access

No default account is registered.
On the logins screen go to the link `or create and account if you don't have one`
and register and account.

# Note
Because of small feature set (compared to redmine) i assume it is mostly used
for personal task tracking, therefore i let the default sqlite in use.

If you want to configure, set this up:
```config.json
{
  "serverRoot": "http://localhost:8000",
  "port": 8000,
  "dbtype": "sqlite3",
  "dbconfig": "./data/focalboard.db",
  "postgres_dbconfig": "dbname=focalboard sslmode=disable",
  "useSSL": false,
  "webpath": "./pack",
  "filespath": "./data/files",
  "telemetry": true,
  "session_expire_time": 2592000,
  "session_refresh_time": 18000,
  "localOnly": false,
  "enableLocalMode": true,
  "localModeSocketLocation": "/var/tmp/focalboard_local.socket"
}
```
and mount to /opt/focalboard/config.json
