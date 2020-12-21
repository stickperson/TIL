# Fixing Slow Local Postgres Connections
tl;dr: use `127.0.0.1` instead of `localhost` or disable ipv6 for `localhost`.


Seemingly out of the blue, I noticed a significant increase in query time when developing in Django. After a bit of digging around in Django's internals, I found that the database connection was taking a while (~26s) to connect. Weird. I then went through the following sequence:

```
> psql
\c <database>
select 1 from <table>;
```
...quick. Ok.

Sanity check:
```
> ./manage.py shell
Model.objects.count()
```
...takes a while.

Digging, digging...

[Github issue](https://github.com/psycopg/psycopg2/issues/916)
```
> export DSN="dbname=dbname host=localhost port=5432 user=<user> password=<pw>"
> time python -c "import os, psycopg2; print(psycopg2.connect(os.environ['DSN']))"

```

I then switched the host from `localhost` to `127.0.0.1` which fixed the issue. Why does this happen?

Well, `/etc/hosts` looks like this:
```
127.0.0.1	localhost
255.255.255.255	broadcasthost
::1             localhost
```

`localhost` can use either ipv4 or ipv6. By default it seems that ipv6 is used which postgres does not like for some reason even though it says it's listening:
```
> tail ~/Library/Application\ Support/Postgres/var-10/postgresql.log
2019-12-04 19:32:10.774 PST [93667] LOG:  worker process: logical replication launcher (PID 93674) exited with exit code 1
2019-12-04 19:32:10.774 PST [93669] LOG:  shutting down
2019-12-04 19:32:10.786 PST [93667] LOG:  database system is shut down
2019-12-04 19:32:11.756 PST [93969] LOG:  listening on IPv6 address "::1", port 5432
2019-12-04 19:32:11.756 PST [93969] LOG:  listening on IPv4 address "127.0.0.1", port 5432
2019-12-04 19:32:11.756 PST [93969] LOG:  listening on Unix socket "/tmp/.s.PGSQL.5432"
2019-12-04 19:32:11.772 PST [93969] LOG:  could not send test message on socket for statistics collector: No buffer space available
2019-12-04 19:32:11.772 PST [93969] LOG:  trying another address for the statistics collector
2019-12-04 19:32:11.775 PST [93970] LOG:  database system was shut down at 2019-12-04 19:32:10 PST
2019-12-04 19:32:11.803 PST [93969] LOG:  database system is ready to accept connections
```

The workaround for now is to either comment out `::1` from `/etc/hosts` or explicitly connect to `127.0.0.1` instead of `localhost`.
