# Server

The Server configuration is managed via [empire/server/config.yaml](https://github.com/BC-SECURITY/Empire/blob/master/empire/client/config.yaml).

Once launched, Empire checks for user write permissions on paths specified in `config.yaml`. If the current user does not have write permissions on these paths, `~/.empire` will be set as fallback parent directory and the configuration file will be updated as well.

* **suppress-self-cert-warning** - Suppress the http warnings when launching an Empire instance that uses a self-signed cert.

* **api** - Configure the RESTful API. This includes the port to run the API on, as well as the path for the SSL certificates. If `empire-priv.key` and `empire-chain.pem` are not found in this directory, self-signed certs will be generated.

```yaml
api:
  port: 1337
  cert_path: empire/server/data/
```

* **database** - Configure Empire's database. Empire defaults to SQLite and has the ability to run with MySQL. For more info on the database, see the [Database](database/README.md) section.

SQLite - The location of the SQLite db file is configurable.

```yaml
database:
  use: sqlite
  sqlite:
    location: empire/server/data/empire.db
```

MySQL - The url, username, password, and database name are all configurable.

```yaml
database:
  use: mysql
  mysql:
    url: localhost
    username:
    password:
    database_name:
```

The defaults block defines the properties that are initially loaded into the database when it is first created.

```yaml
database:
  defaults:
    # staging key will first look at OS environment variables, then here.
    # If empty, will be prompted (like Empire <3.7).
    staging-key: RANDOM
    username: empireadmin
    password: password123
    # The default configuration for global obfuscation.
    obfuscation:
      - language: powershell
        enabled: false
        command: "Token\\All\\1"
        module: "invoke-obfuscation"
        preobfuscatable: true
      - language: csharp
        enabled: false
        command: ""
        module: "confuser"
        preobfuscatable: false
    # an IP white list to ONLY accept clients from
    #   format is "192.168.1.1,192.168.1.10-192.168.1.100,10.0.0.0/8"
    ip-whitelist: ""
    # an IP black list to reject accept clients from
    #   format is "192.168.1.1,192.168.1.10-192.168.1.100,10.0.0.0/8"
    ip-blacklist: ""
    # Adds keywords that will be obfuscated in Empire. For example, anytime
    # Invoke-Empire or Invoke-Mimikatz is used in a module/stager, it will
    # be replaced with a random 5 character string.
    keyword_obfuscation:
      - Invoke-Empire
      - Invoke-Mimikatz
```

* **plugins** - Auto runs plugins with defined settings. This tells Empire to run a set of commands with the plugin at server startup.

```
plugins:
  # Auto-execute plugin with defined settings
  csharpserver:
    status: start
```

* **directories** - Control where Empire should read and write specific data.

```
directories:
  downloads: empire/server/downloads/
  module_source: empire/server/data/module_source/
  obfuscated_module_source: empire/server/data/obfuscated_module_source/
```

* **logging** - See [Logging](../../logging/logging.md) for more information on logging configuration.

* **submodules** - Control if submodules wil be auto updated on startup.

```
submodules:
  auto_update: true
```