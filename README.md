# jklik

1.	From the enterprisedb OS user
mkdir /PostgresPlus/backup/hostname/receivexlog_wals
chmod 700 /PostgresPlus/backup/hostname/receivexlog_wals

2.	change max_replication_slots in /PostgresPlus/data/postgresql.conf
max_replication_slots = 1
PostgreSQL Service restart is required after doing this change in postgresql.conf

3.	#create a replication slot
pg_receivexlog --directory=/PostgresPlus/backup/hostname/receivexlog_wals --no-loop --create-slot --slot=receivexlog_wals_slot --synchronous --verbose &>  /PostgresPlus/backup/hostname/receivexlog_wals/receivexlog.log

4.	run this in the foreground.
pg_receivexlog --directory=/PostgresPlus/backup/hostname/receivexlog_wals --no-loop  --slot=receivexlog_wals_slot --synchronous --verbose &> /PostgresPlus/backup/hostname/receivexlog_wals/receivexlog.log &

5.	to clean all the WAL's in receivexlog_wals directory
pg_archivecleanup /PostgresPlus/backup/hostname/receivexlog_wals `cd /PostgresPlus/backup/hostname/receivexlog_wals ; ls -lt *.partial`


