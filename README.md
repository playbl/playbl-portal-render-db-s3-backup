# Playbl Portal Render DB backup cron service

This repo configures a [Render cron job](https://render.com/docs/cronjobs) that stores nightly snapshots of the live Playbl Portal DB to a private S3 bucket. Forked from https://github.com/render-examples/postgres-s3-backups, based on this guide https://render.com/docs/backup-postgresql-to-s3.

Deploying this involved creating a "Blueprint" on Render, which I then used to create a "Blueprint Instance" which is the cronjob itself. I deleted the Blueprint to reduce confusion after creating the cronjob.

Links:

- This Github repo: https://github.com/playbl/playbl-portal-render-db-s3-backup
- The main Playbl Portal app repo: https://github.com/playbl/phoenix-portal
- Render cron job: https://dashboard.render.com/cron/crn-d32lu2juibrs739vacrg

## How to deploy changes

- Make changes, commit to the `main` branch, push to Github.
- The Render `playbl-portal-prod-db-s3-backup` service should detect the change and rebuild.
- Go to the cron job on Render, ensure the build succeeded, then click "Trigger Run" to test.

## How to download a DB snapshot

- Ensure you have the same or newer Postgres version locally (v17).
- Log into Playbl's main AWS account, go to S3 bucket `playbl-portal-db-snapshots`.
- Locate the snapshot, download it to a secure confidential folder, unzip it.
- Confirm that the data loads cleanly into a temporary local DB:
  - `createdb playbl_prod_temp`
  - `psql -d playbl_prod_temp -f backup-2025-09-13-124104.sql`
  - {open the DB, confirm it has correct-looking data}
  - `dropdb playbl_prod_temp`
