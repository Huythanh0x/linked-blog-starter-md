### Run local
#### Tes versions
```sh
node -v

pnpm -v

docker --version

docker compose version
```

#### Start local DB
```sh
docker compose up -d --build
```

#### Dependencies
```sh
pnpm install
```

#### Run migrations
```sh
pnpm --filter @repo/schema migrate:local
pnpm --filter @repo/schema seed:master:local
```

#### Start all apps
```sh
pnpm dev:local
```