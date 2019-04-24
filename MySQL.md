# WAL-G MySQL extension

This extension allows you to use wal-g as a tool for encrypting, compressing MySQL backups and push/fetch them to/from storage without saving it on your filesystem.

Configuration
-------------

* `WALG_MYSQL_DATASOURCE_NAME`

To configure connection string for MySQL. Format ```user:password@host/dbname```

* `WALG_MYSQL_BINLOG_DST`

To place binlogs in the specified directory during stream-fetch.

* `WALG_MYSQL_BINLOG_SRC`

To configure directory with binlogs for ```binlog-push```.

* `WALG_MYSQL_BINLOG_END_TS`

To set time [RFC3339](https://www.ietf.org/rfc/rfc3339.txt) for recovery point.

* `WALG_MYSQL_SSL_CA`

To use SSL, a path to file with certificates should be set to this variable.


Usage
-----

WAL-G mysql extension currently supports these commands:


* ``stream-fetch``

When fetching backup's stream, the user should pass in the name of the backup. It returns an encrypted data stream to stdout, you should pass it to a backup tool that you used to create this backup.
```
wal-g mysql stream-fetch example-backup | some_backup_tool use_backup
```
WAL-G can also fetch the latest backup using:

```
wal-g mysql stream-fetch LATEST | some_backup_tool use_backup
```

* ``stream-push``

Command for compressing, encrypting and sending backup from stream to storage.

```
some_backup_tool make_backup | wal-g mysql stream-push
```

* ``binlogs-push``

Command for sending binlogs to storage by CRON.

```
wal-g mysql binlog-push /path/to/binlogs
```

* ``delete retain``

Saves retention_count number latest backups and delete other. See detailed description at the main readme. Usage:
```
wal-g mysql delete retain [FULL|FIND_FULL] retention_count
```

* ``delete before``

Delete backups before name or time. See detailed description at the main readme. Usage:
```
wal-g mysql delete before [FIND_FULL] backup_name|time
```
