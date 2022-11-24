# Mastodon

This is a [BastilleBSD](https://bastillebsd.org) Template to install Mastodon on your Server. 
Find out more about *BastilleBSD* by reading the [documentation](https://bastille.readthedocs.io/en/latest/).

# Prerequisite

You will need access to a PostgreSQL server. 

Login to the database server and create the user for your Mastodon instance as described in Mastodon's 
Documentation [here](https://docs.joinmastodon.org/admin/install/#setting-up-postgresql).

Add a new entry to ```sh /var/db/postgres/data14/pg_hba.conf``` ( Version 14 = data14 )

```sh
host    mastodon        mastodon        10.0.0.10/32            trust
```

If you use [BastilleBSD](https://bastillebsd.org) to manage your Jails you can do it similar to this.
Create a new Jail and then bootstrap this [yaazkal/bastille-postgres](https://github.com/yaazkal/bastille-postgres)
BastilleBSD template.


```sh
bastille create postgres 13.1-RELEASE 10.0.0.10
bastille bootstrap https://github.com/yaazkal/bastille-postgres
bastille template yaazkal/bastille-postgres
bastille cmd postgres su - postgres -c "psql -c 'CREATE USER mastodon CREATEDB;'"
bastille cmd postgres sh -c 'echo "host    mastodon        mastodon        10.0.0.10/32            trust" >> /var/db/postgres/data14/pg_hba.conf'
```

Read [Installing Mastodon from source](https://docs.joinmastodon.org/admin/install/#creating-a-user)

Furthermore [Mastodon](https://joinmastodon.org/) depends on having [Redis](https://redis.io/) ready
On default the Redis Server Package is bundled with this Jail (Container). 
 
# Bootstrap

```sh
bastille bootstrap https://codeberg.org/ddowse/mastodon
```

```sh
bastille template TARGET ddowse/mastodon --arg DOMAIN=full.quailified.domainname --arg EMAIL=mailbox@example.org
```
