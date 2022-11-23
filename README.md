# mastodon

This is a BastilleBSD Template to bootstrap Mastodon on your Server. PostgreSQL not included. 

# Bootstrap

```sh
bastille bootstrap https://codeberg.org/ddowse/mastodon
```

```sh
bastille template TARGET ddowse/mastodon --arg DOMAIN=full.quailified.domainname --arg EMAIL=mailbox@example.org
```

# Prerequisite

Working PostgreSQL e.g [yaazkal/bastille-postgres](https://github.com/yaazkal/bastille-postgres)

Read [Installing Mastodon from source](https://docs.joinmastodon.org/admin/install/#creating-a-user)

