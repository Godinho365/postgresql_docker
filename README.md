# docker_postgresql

Ambiente Docker com PostgreSQL 17 + PostGIS e pgAdmin para restaurar e explorar o banco `pm`.

## Pré-requisitos (Ubuntu)

```bash
# Docker Engine + Compose plugin
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
newgrp docker   # ou faça logout/login para aplicar o grupo docker

docker --version
docker compose version
```

## Instalação

```bash
# 1. Copie o projeto para a máquina (git clone, scp, pendrive, etc.)
git clone <url-do-repositorio> docker_postgresql
cd docker_postgresql

# 2. Coloque o dump pm.sql na raiz do projeto (não vem no git, é grande)
#    ex.: scp usuario@origem:/caminho/pm.sql .

# 3. Suba os containers
docker compose up -d

# 4. Aguarde o banco ficar "healthy"
docker compose ps
```

## Restaurar o banco `pm`

```bash
docker exec pm_postgis psql -U postgres -d pm -f /restore/pm.sql
```

O erro `invalid command \unrestrict` no final da restauração é esperado (meta-comando de segurança do `pg_dump` 17) e não indica falha nos dados.

## Verificação

```bash
docker exec pm_postgis psql -U postgres -d pm -c "\dt edgv.*"
docker exec pm_postgis psql -U postgres -d pm -c "SELECT postgis_version();"
docker exec pm_postgis psql -U postgres -d pm -c "SELECT count(*) FROM information_schema.tables WHERE table_schema IN ('edgv','dominios');"
```

## Acesso

| Serviço  | Endereço                          | Usuário            | Senha      |
|----------|------------------------------------|---------------------|------------|
| Postgres | `<ip-da-maquina>:5432`, banco `pm` | `postgres`          | `postgres` |
| pgAdmin  | `http://<ip-da-maquina>:5050`     | `admin@postgres.com`| `postgres` |

O `pg_hba.conf` libera acesso de qualquer host da rede (`0.0.0.0/0`). Se o firewall do Ubuntu (`ufw`) estiver ativo, libere as portas:

```bash
sudo ufw allow 5432/tcp
sudo ufw allow 5050/tcp
```

## Parar / remover

```bash
docker compose down          # para os containers, mantém os dados (volume pgdata)
docker compose down -v       # para e apaga também os dados do banco
```
