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