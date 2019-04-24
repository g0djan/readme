## WAL-G for MySQL

You can use wal-g as a tool for encrypting, compressing MySQL backups and push/fetch them to/from storage without saving it on your filesystem.

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
wal-g stream-fetch example_backup | xbstream -x -C mysql_datadir
```
WAL-G can also fetch the latest backup using:

```
wal-g stream-fetch LATEST | xbstream -x -C mysql_datadir
```

* ``stream-push``

Command for compressing, encrypting and sending backup from stream to storage.

```
xtrabackup --backup --stream=xbstream --datadir=mysql_datadir | wal-g stream-push
```

* ``binlog-push``

Command for sending binlogs to storage by CRON.

```
wal-g binlog-push
```
