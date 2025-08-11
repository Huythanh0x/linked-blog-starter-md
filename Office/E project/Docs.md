### Server
[[Simple email service]] 62k email free when sending from application running in [[Amazon EC2]]

### DB design 
[[Database design]]
#### List tables
- ~~**Service Owner**(login, avatar, userinfo, )~~
- ~~**Tenant owner**(tenant info, )~~
- ~~**corporations** (compnany name, paid date, payment status this period, , address, name of presenter, phoneNumber, mail, signged up date)~~
- ~~**CoporationPlans**(paydate, payment amount, status, invoice, current user amount, user signed up, the amount payment, next payment date)~~
- ~~**Tenant connection** -> Follow the sample~~

- **Logs** (recent activity{ new corporation, new user})

- ~~**User** (status, name, email, signedupdate,~~ feedback count)
- ~~**History Training Scores** (score, date taken)~~
- ~~**Training** (type: {CHAT/VOICE}, level {4 levels}, title, description)~~
### DB setup 
#### Migration script
```sh
yarn run typeorm -- -d ./typeOrm.config.ts migration:generate ./src/migrations/root/updateRootDBForRolePlaying
```

```sh
yarn run typeorm -- -d ./tenant.typeOrm.config.ts migration:generate ./src/migrations/tenant/updateTenantDBForRolePlaying
```

#### Check DB creds
```sh
docker-compose ps
# Check environment variables in the container
docker exec -it Sales-Role-Playing-postgres env | grep POSTGRES
```

#### List out all schemas
```sh
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "\dn;"
```
#### Inline query all service-owners

```sh
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "SELECT * FROM service_owners;"
```

#### Inline query all owners (default-tenant)

```sh
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "SELECT * FROM \"enjoyworks_tenant\".owners;"
```

#### Inline query all users (default-tenant)

```sh
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "SELECT * FROM \"enjoyworks_tenant\".users;"
```

#### List out all tables (default-tenant)
```sh
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "SELECT table_name FROM information_schema.tables WHERE table_schema = 'enjoyworks_tenant' ORDER BY table_name;"
```

#### List all migrations
```sh
docker exec -it SalesRolePlayingAPI-postgres psql -U postgres -d medimony -c "SELECT * FROM "migrations";"
```

#### Remove all migrations
```sh
docker exec -it SalesRolePlayingAPI-postgres psql -U devUser -d db_template -c "DELETE FROM "migrations""
```

#### list out all tables count (check not cleared test data)
```sh
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "

SELECT 'users' as table_name, COUNT(*) as row_count FROM \"enjoyworks_tenant\".users

UNION ALL
SELECT 'trainings', COUNT(*) FROM \"enjoyworks_tenant\".trainings

UNION ALL
SELECT 'training_details', COUNT(*) FROM \"enjoyworks_tenant\".training_details

UNION ALL
SELECT 'products', COUNT(*) FROM \"enjoyworks_tenant\".products

UNION ALL
SELECT 'training_role_personality', COUNT(*) FROM \"enjoyworks_tenant\".training_role_personality

UNION ALL
SELECT 'owners', COUNT(*) FROM \"enjoyworks_tenant\".owners

ORDER BY table_name;"
```

# Feature
### Login
- Integrate the Google Auth Service
- Address the use case and activity flow
	- SuperAdmin: migration script to create first SuperAdmin. Then can have role (Admin, Super Admin) at **Service Owner**. Validate of certain domain
	- **TenantOwner**: login via Google Auth. Both are under the same table row
	- **User**: login via email-password or Google Auth. Both are under the same table row
- Update DB docs for these 3 tables
- Update entities for 3 tables
- Add **3 different flows** for each user 
	- *xxx*.graphql
	- resolver
	- service
	- dto
	- strategy + guard
- Unit-test
	- service
	- guard
- E2E test
- Manual test (postman, create test UI)
- Create Pull Request, resolve PR comments
- Fix bug
> Estimation
- Google Auth integration: 2h