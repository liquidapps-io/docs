PostgreSQL Database Backend
========

A PostgreSQL databse is utilized to prevent duplicate DSP transactions by creating, getting, and updating service events as needed.  An external database can be set by ensuring the `node_env` variable in the `config.toml` file is set to `production`.  The database settings may be specified with the `url` variable, e.g., `postgres://user:pass@example.com:5432/dbname`.

### config.toml:

```bash
[database]

# url syntax: postgres://user:pass@example.com:5432/dbname, only necessary for production

url = "postgres://user:pass@example.com:5432/dbname" 

# production (uses above database_url for database)

node_env = "production"
```

### [How to install postgres on Ubuntu](https://computingforgeeks.com/install-postgresql-12-on-ubuntu/)

```bash
wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -
echo "deb http://apt.postgresql.org/pub/repos/apt/ `lsb_release -cs`-pgdg main" |sudo tee  /etc/apt/sources.list.d/pgdg.list
sudo apt update
sudo apt -y install postgresql-12 postgresql-client-12
```

### Setup database and user

```bash
sudo su - postgres
psql
CREATE DATABASE dsp;
CREATE USER dsp WITH ENCRYPTED PASSWORD 'Put-Some-Strong-Password-Here';
GRANT ALL PRIVILEGES ON DATABASE dsp to dsp;
```

### How to wipe local database

```bash
sudo su - postgres
psql
\l  # to list database and find your DB name, mine is "dsp"
\c dsp #to connect to the DB
drop table "Settings";
drop table "ServiceRequests";
<CTRL> d # exit
```