# Keemail

Never expose your personal email again

- [Keemail](#keemail)
  * [About / Synopsis](#about---synopsis)
  * [Installation](#installation)
    + [Basic Install](#basic-install)
    + [Docker](#docker)
      - [DNS](#dns)
      - [Database](#database)
      - [Keemail Server](#keemail-server)
      - [Run](#run)
  * [Screeshots](#screeshots)



## About / Synopsis

Keemail is a solution that allows you to easily create custom aliases for an email address through a webpage. It also come with a REST API that lets your users ingrate Keemail into theirs favorites tools. It's using Flask for the webserver and Postfix for the mailserver. The main goal of the API is to integrate Keemail into Passwords Manager Software like Keepass so you don't have to bother to manually create the alias.



TL;DR

* Keemail allows you to easily generate aliases to your personal email address
* Provide a REST API to let you integrate it into your favorite tools
* Aliases are randomly generated but can be customized

## Installation

You can install Keemail manually but we'd recommend using Docker to automate deployment

```bash
$ git clone https://github.com/Guisch/keemail
$ cd keemail
```





### Basic Install

1. Create a MySQL Database & user having full access to this database
2. Visit the [Keemail Server](https://github.com/Guisch/keemailserver) Repo and follow the guide
3. Visit the [Keemail Postfix](https://github.com/Guisch/keemailpostfix) Repo and follow the guide



### Docker

You can automate deployment with Docker.

#### DNS

You first need to own a domain. You can buy one on domais.google, godaddy, ovh, etc or create a free one on freenom. Once you own your domain, you need to create an MX redirection to the server that will run the postfix container. Please refer to your registrar to know how to do it. Once it's done, edit the Keemail Postfix and Keemail Server domain on line 25 and 41 of docker-compose.yml

```yaml
           [...]
  25       KEEMAIL_DOMAIN: "example.com"
           [...]
  41       DOMAIN: "example.com"
```



#### Database

A MariaDB container is by default installed in the docker-compose.yml. If you want to use your own database please remove all lines under keemaildb (lines 3 to 12) and remove links and depends_on (16 to 19, 30 to 33). Then change the DB infos on line 24 (KEEMAIL_DB_URI) and lines 37 to 40 (MYSQL_*).

If you are using the provided MariaDB container, please change the password of your keemail user line 12, 24 and 38 of docker-compose.yml

```yaml
		   [...]
  12       MYSQL_PASSWORD: "pouetpouet"
  		   [...]
  24       KEEMAIL_DB_URI: "mysql://keemail:pouetpouet@keemaildb:3306/keemail"
           [...]
  38       MYSQL_PASSWORD: "pouetpouet"
  		   [...]
```



#### Keemail Server

Please make sure to change the KEEMAIL_SECRETKEY on line 23 (used for Flask Sessions). If you are running Keemail in production, change the Flask Env in line 26 to `production`

```yaml
  		   [...]
  23       KEEMAIL_SECRETKEY: "pouetpouet"
  		   [...]
  26       KEEMAIL_ENV: "development"
  		   [...]
```



#### Run

Using docker-compose, you create first the database, then the webserver and to finish the mailserver

```bash
$ docker-compose up -d keemaildb
$ docker-compose up -d keemailserver
$ docker-compose up -d keemailpostfix
```



## Screeshots

[WIP]