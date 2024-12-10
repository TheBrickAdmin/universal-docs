---
description: psucli is a command line utility for working with PowerShell Universal.
---

# psucli

`psucli` is included with the PowerShell Universal installation media.&#x20;

## db

Commands for working with the PowerShell Universal database.&#x20;

### convert

Convert a v4 LiteDB Database to SQLite

```
psucli db convert --path C:\ProgramData\UniversalAutomation\databased.db
```



| Argument | Description                         | Required |
| -------- | ----------------------------------- | -------- |
| --path   | The path of the database to convert | ✅        |

### schema

Migrate from one schema version to another.  Migrating to lower versions can cause data loss.

```
psucli db schema --connection-string 'Server=SQL;Data Source=PSU;Integrated Security=True' --schema-version 5.1.0 --database-type 'SQL'
```



| Argument            | Description                                       | Required |
| ------------------- | ------------------------------------------------- | -------- |
| --connection-string | Connection string to the database                 | ✅        |
| --schema-version    | The database schema version. Defaults to "Latest" | ❌        |
| --database-type     | PostgreSQL, SQL or SQLite (default)               | ❌        |

### Migrate

Migrates from one database to another. This command can migrate between database types.

```
psucli db migrate --source-connection-string 'Server=SQL;Data Source=PSU;Integrated Security=True' -source-database-type 'SQL' --target-connection-string 'Server=PostgreSQL;Data Source=PSU;Integrated Security=True' --target-database-type 'PostgreSQL'
```



| Argument                   | Description                         | Required |
| -------------------------- | ----------------------------------- | -------- |
| --target-connection-string | Target database connection string   | ✅        |
| --source-connection-string | Source database connection string   | ✅        |
| --target-database-type     | PostgreSQL, SQL or SQLite (default) | ❌        |
| --source-database-type     | PostgreSQL, SQL or SQLite (default) | ❌        |

## git

Commands for working with PSU git repositories. This command uses the internal git services to work with the local repository.&#x20;

### clone

Clones a git repository using the git sync service.&#x20;

```
psucli git clone --url http://github.com/ironmansoftware/psu.git --path C:\ProgramData\UniversalAutomation\Repository --username 'adamdriscoll' --password 'gh__1234123' --branch main
```



| Argument   | Description                            | Required |
| ---------- | -------------------------------------- | -------- |
| --url      | The URL of the git remote.             | ✅        |
| --path     | The local path to clone to             | ✅        |
| --username | The user name for the remote.          | ❌        |
| --password | The password for the remote            | ❌        |
| --branch   | The branch to clone (default is main). | ❌        |

### pull

Pulls from a git remote. A clone will be called if the local repository does not exist.

```
psucli git pull --url http://github.com/ironmansoftware/psu.git --path C:\ProgramData\UniversalAutomation\Repository --username 'adamdriscoll' --password 'gh__1234123' --branch main
```

| Argument   | Description                            | Required |
| ---------- | -------------------------------------- | -------- |
| --url      | The URL of the git remote.             | ✅        |
| --path     | The local path to clone to             | ✅        |
| --username | The user name for the remote.          | ❌        |
| --password | The password for the remote            | ❌        |
| --branch   | The branch to clone (default is main). | ❌        |

### push

Pushes to a git remote. The repository needs to be cloned first. Changes will not be staged during the push.

```
psucli git push --url http://github.com/ironmansoftware/psu.git --path C:\ProgramData\UniversalAutomation\Repository --username 'adamdriscoll' --password 'gh__1234123' --branch main
```

| Argument   | Description                            | Required |
| ---------- | -------------------------------------- | -------- |
| --url      | The URL of the git remote.             | ✅        |
| --path     | The local path to clone to             | ✅        |
| --username | The user name for the remote.          | ❌        |
| --password | The password for the remote            | ❌        |
| --branch   | The branch to clone (default is main). | ❌        |
