# docker_postgresql

Ambiente Docker com PostgreSQL 17 + PostGIS e pgAdmin, usado para restaurar e explorar o banco `pm` (dump em `pm.sql`, schemas `dominios` e `edgv`, extensões `postgis`/`hstore`/`uuid-ossp`).

## Estrutura
- `docker-compose.yml` — serviços `db` (postgis/postgis:17-3.4) e `pgadmin` (dpage/pgadmin4).
- `config/postgresql.conf` e `config/pg_hba.conf` — liberam acesso ao Postgres em toda a rede local (uso em ambiente de desenvolvimento/rede confiável).
- `pm.sql` — dump a restaurar no banco `pm` (não versionado no git, arquivo grande).

## Credenciais (dev)
- Postgres: usuário `postgres` / senha `postgres`, banco `pm`, porta `5432`.
- pgAdmin: `admin@postgres.com` / `postgres`, porta `5050`.

## Convenção de commits
Toda alteração de configuração/infra gera um commit descritivo e reversível (`git revert`/`git reset` disponíveis no histórico).
