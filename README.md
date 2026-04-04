# Wikijs on K3S
## install postgreSQL
```bash
cd
mkdir git
cd git
git clone https://github.com/joe-speedboat/kube.postgresql.git
cd kube.postgresql
bash deploy_postgresql.sh
```
* create namepace as needed, forex: wikijs
* enter, enter, finish
* check deployment
  ```bash
  kubectl get all
  ```
# Install WikiJS
```bash
# must be same as you deployed db
NS=wikijs-test
FQDN=wiki.domain.tld

sed "s/__NS__/$NS/" wikijs.yml | sed "s/__FQDN__/$FQDN/" | kubectl apply -f -

```

# Install sitemap generator and ingress
```bash
sed "s/__NS__/$NS/" wikijs_sitemap.yml | sed "s/__FQDN__/$FQDN/" | kubectl apply -f -
sed "s/__NS__/$NS/" wikijs_sitemap_ingress.yml | sed "s/__FQDN__/$FQDN/" | kubectl apply -f -
```

# Setup PGSQL fulltext search
* https://docs.requarks.io/en/search/postgres
```bash

kubectl exec -it deployment.apps/postgresql -- /bin/bash

PGPASSWORD="$POSTGRES_PASSWORD" \
psql -U "$POSTGRES_USER" -d "$POSTGRES_DB"

CREATE EXTENSION pg_trgm;

```
