# Mastodon

This is a [BastilleBSD](https://bastillebsd.org) template to bootstrap [Mastodon](https://joinmastodon.org) in a FreeBSD jail. Find out more about *BastilleBSD* by reading the [documentation](https://bastille.readthedocs.io/en/latest/).

A tutorial how to install Mastodon on FreeBSD can be found in Stefano Marinelli's Website [Installing Mastodon inside a FreeBSD jail](https://it-notes.dragas.net/2022/11/23/installing-mastodon-on-a-freebsd-jail/).

The ```rc(8)``` scripts needed to start the services at boottime are taken from Stefano Marinelli's fine tutorial. Kudos!

```sh
bastille bootstrap https://codeberg.org/ddowse/mastodon
```

```sh
bastille template TARGET ddowse/mastodon --arg DOMAIN=mastodon.example.org --arg EMAIL=mailbox@example.org 
```


# Detailed beginners description   

# Prerequisite (Software)

## PostgreSQL

You will need access to a [PostgreSQL](https://www.postgresql.org) Server. 

Login to the database server and create the user for your Mastodon instance as described in Mastodon's 
Documentation [here](https://docs.joinmastodon.org/admin/install/#setting-up-postgresql).

Add a new entry to your ```pg_hba.conf```

#### Example! 
```sh
host    mastodon        mastodon        10.0.0.10/32            trust
```    


In case you don't have a running PostgreSQL then just create a new Jail and bootstrap this 
[yaazkal/bastille-postgres](https://github.com/yaazkal/bastille-postgres) BastilleBSD template.

#### Example! 

```sh
bastille create postges 13.1-RELEASE 10.0.0.10
bastille bootstrap https://github.com/yaazkal/bastille-postgres
bastille template yaazkal/bastille-postgres
bastille cmd postgres su - postgres -c "psql -c 'CREATE USER mastodon CREATEDB;'"
bastille cmd postgres sh -c 'echo "host    mastodon        mastodon        10.0.0.10/32            trust" >> /var/db/postgres/data14/pg_hba.conf'
```
## Redis

Mastodon needs access to a [Redis](https://redis.io/) server. Redis Server is installed as a dependency anyway so to simplify things for beginners the *redis* daemon will run in *unprotected mode*. Please check the ```Bastillefile``` for more details. 

In the mastodon setup process enter the jails IP instead of localhost.

## TLS certificate

Make sure ```Port 80/443``` are open. Port 80 is only needed to get a TLS certificate from [Let's Encrypt](https://letsencrypt.org).

# Bootstrap 

```sh
bastille bootstrap https://codeberg.org/ddowse/mastodon
```

This nextstep will run the commands found in the **Bastillefile**
It is wise to create a *zfs snapshot* of the jail before applying it. 
In case something goes wrong you can easily rollback. 
Read more about ZFS snapshots on the FreeBSD [Documentation](https://docs.freebsd.org/en/books/handbook/zfs/#zfs-term-snapshot)

```sh
bastille template TARGET ddowse/mastodon --arg DOMAIN=mastodon.example.org --arg EMAIL=mailbox@example.org
```
