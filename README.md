# postgres-auto-backup-s3

## Important

This is a forked repository with small changes from [here](https://github.com/karser/docker-images)

Backup PostgresSQL to S3 (supports periodic backups)

This is a fork of schickling/postgres-backup-s3 with postgres 12.4 support.

## Usage

Docker:
```sh
$ docker run -e S3_ACCESS_KEY_ID=key -e S3_SECRET_ACCESS_KEY=secret -e S3_BUCKET=my-bucket -e S3_PREFIX=backup -e POSTGRES_DATABASE=dbname -e POSTGRES_USER=user -e POSTGRES_PASSWORD=password -e POSTGRES_HOST=localhost erfan37/postgres-auto-backup-s3
```

Docker Compose:
```yaml
postgres:
  image: postgres
  environment:
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password

pgbackups3:
  image: karser/postgres-backup-s3
  links:
    - postgres
  environment:
    SCHEDULE: '@daily'
    S3_REGION: region
    S3_ACCESS_KEY_ID: key
    S3_SECRET_ACCESS_KEY: secret
    S3_BUCKET: my-bucket
    S3_PREFIX: backup
    POSTGRES_DATABASE: dbname
    POSTGRES_USER: user
    POSTGRES_PASSWORD: password
    POSTGRES_EXTRA_OPTS: '--schema=public --blobs'
    ZIP_DUMP: 1
```

### Automatic Periodic Backups

You can additionally set the `SCHEDULE` environment variable like `-e SCHEDULE="@daily"` to run the backup automatically.

Also, you can set the `ZIP_DUMP` to `0` or `1` to choose whether you want to zip the output file or not.

You can set the `POSTGRES_DATABASE` environment to `DUMPALL` to perform `pg_dumpall` and get a full backup from the whole database.
While setting `POSTGRES_DATABASE=DUMPALL`, please note that you need to set proper `username` and `password` that has access to all databases.

More information about the scheduling can be found [here](http://godoc.org/github.com/robfig/cron#hdr-Predefined_schedules).
