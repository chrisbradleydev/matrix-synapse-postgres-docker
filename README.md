# matrix-synapse-postgres-docker

Clone the repository.

```sh
git clone https://github.com/chrisbradleydev/matrix-synapse-postgres-docker.git .
```

Copy and update env files.

```sh
cp ~/reverse-proxy/.env.example ~/reverse-proxy/.env
cp ~/synapse/.env.example ~/synapse/.env
```

Create `matrix` Docker network.

```sh
sudo docker network create matrix
```

Generate Synapse default configuration.

```sh
cd synapse
sudo docker compose run --rm synapse generate
```

Update `~/synapse/data/homeserver.yaml` to use PostgreSQL.

```yaml
database:
  name: psycopg2
  args:
    user: user
    password: password
    host: postgresql
    database: synapse
    cp_min: 5
    cp_max: 10
```

Run with Docker Compose.

```sh
sudo docker compose \
  -f ~/reverse-proxy/docker-compose.yml \
  -f ~/synapse/docker-compose.yml up -d
```
