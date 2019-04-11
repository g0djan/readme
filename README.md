# WAL-G MySQL extension

This extension allows you to use wal-g as a tool for encrypting, compressing and sending MySQL backups to storage without saving it on your filesystem.

Configuration
-------------
**Required**

WAL-G MySQL uses environment variables

To configure path to MySQL data
* `WALG_MYSQL_DATASOURCE_NAME`

To store binlogs in specified directory
* `WALG_MYSQL_BINLOG_DST`

To set time for recovery point
* `WALG_MYSQL_BINLOG_END_TS` 

To use SSL, path to file with certificates should be set to this variable
* `WALG_MYSQL_SSL_CA`


Usage
-----

WAL-G mysql extension currently supports these commands:


* ``stream-fetch``

When fetching backup's stream, the user should pass in the name of the backup. It returns encrypted data stream to stdout, you should pass it to backup tool that you used for create this backup.
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

Command saves retention_count number latest backups and delete other. See more detailed description at main readme. Usage:
```
wal-g mysql delete retain [FULL|FIND_FULL] retention_count
```

* ``delete before``

Command delete backups before name or time. See more detailed description at main readme. Usage:
```
wal-g mysql delete before [FIND_FULL] backup_name|time
```
