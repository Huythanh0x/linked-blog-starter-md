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
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "SELECT * FROM \"thanhvh_hello_vitalify_asia_4171c59139\".trainings;"
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
docker exec -it Sales-Role-Playing-postgres psql -U postgres -d enjoywork -c "SELECT * FROM "migrations";"
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
SELECT 'chat_messages', COUNT(*) FROM \"enjoyworks_tenant\".chat_messages

UNION ALL
SELECT 'corporations', COUNT(*) FROM \"enjoyworks_tenant\".corporations


ORDER BY table_name;"
```

#### Create test data for training and training details
**Add 5 users**
```sh
docker exec -i Sales-Role-Playing-postgres psql -U postgres -d enjoywork <<'SQL'
SET search_path TO "enjoyworks_tenant";

-- Create 5 demo users if not existing
INSERT INTO users ("email","password","name","status","feedBackCount","lastLogin","isVerified")
SELECT 'demo_user_'||i||'@example.com', NULL, 'Demo User '||i, 1, 0, now(), 1
FROM generate_series(1,5) AS g(i)
ON CONFLICT ("email") DO NOTHING;

-- Verify
SELECT "userId","email","createdAt" FROM users WHERE email LIKE 'demo_user_%@example.com' ORDER BY "email";
SQL
```
**Add 10 trainings for each user**
```sh
docker exec -i Sales-Role-Playing-postgres psql -U postgres -d enjoywork <<'SQL'
SET search_path TO "enjoyworks_tenant";

-- Insert 10 trainings per demo user
WITH new_trainings AS (
  INSERT INTO trainings ("trainingType","trainingMode","level","topic","userId")
  SELECT
    0,                               -- CHAT
    (j % 2),                         -- 0: Persuasion, 1: Negotiation
    (j % 4),                         -- 0..3: Basic..Demon
    'Seed Topic #'||j,
    u."userId"
  FROM (SELECT "userId" FROM users WHERE email LIKE 'demo_user_%@example.com') u
  CROSS JOIN generate_series(1,10) AS j
  RETURNING "trainingId","userId"
),
insert_details AS (
  INSERT INTO training_details (
    "status","score","scoreQuestion","scoreClosing","scoreLogical",
    "scoreSuggestion","scoreListening","trainingId","overallEvaluation"
  )
  SELECT
    2,                                        -- COMPLETED
    ROUND((random()*40 + 60)::numeric, 2),    -- overall score: 60..100
    ROUND((random()*10)::numeric, 2),
    ROUND((random()*10)::numeric, 2),
    ROUND((random()*10)::numeric, 2),
    ROUND((random()*10)::numeric, 2),
    ROUND((random()*10)::numeric, 2),
    t."trainingId",
    'Auto seeded'
  FROM new_trainings t
  RETURNING "trainingDetailId","trainingId"
)
-- Link details back to training
UPDATE trainings tr
SET "trainingDetailId" = d."trainingDetailId"
FROM insert_details d
WHERE d."trainingId" = tr."trainingId";

-- Quick sanity checks
SELECT COUNT(*) AS total_users FROM users WHERE email LIKE 'demo_user_%@example.com';
SELECT COUNT(*) AS total_trainings FROM trainings t JOIN users u ON u."userId"=t."userId" WHERE u.email LIKE 'demo_user_%@example.com';
SELECT COUNT(*) AS total_details FROM training_details d JOIN trainings t ON t."trainingId"=d."trainingId" JOIN users u ON u."userId"=t."userId" WHERE u.email LIKE 'demo_user_%@example.com';
SQL
```

### Query all Trainings and Details by email
```sql
SELECT
t."trainingId",
td.status,
td.score,
(td."scoreQuestion" + td."scoreClosing" + td."scoreLogical" + td."scoreSuggestion" + td."scoreListening") / 5.0 as average_detailed_score,
td.*
FROM users u
JOIN trainings t ON t."userId" = u."userId"
LEFT JOIN training_details td ON td."trainingId" = t."trainingId"
WHERE u.email = 'tuantm+1@vitalify.asia'
AND (td.status != 0 OR td.status IS NULL);
```

### Query user DB by email
```SQL
	SELECT tenant_owners.*
	FROM tenant_owners
	JOIN memberships ON memberships."tenantOwnerId" = tenant_owners."tenantOwnerId"
	JOIN members ON members."memberId" = memberships."memberId"
	WHERE members.email = 'tuantm+1@vitalify.asia';
```

### Delete user/tenant (member,  membership, users and related data to users)
#### Delete member and memberships (tenant and user)
```SQL
-- SQL Script to safely delete members and their memberships by email addresses

-- Emails: trangiahon92@gmail.com, hontg+1@vitalify.asia

-- This script uses transactions to ensure data integrity

  

START TRANSACTION;

  

-- First, let's check if the members exist

SELECT

m."memberId",

m."email",

COUNT(ms."membershipId") as membership_count

FROM "members" m

LEFT JOIN "memberships" ms ON m."memberId" = ms."memberId"

WHERE m."email" IN ('trangiahon92@gmail.com', 'hontg+1@vitalify.asia')

GROUP BY m."memberId", m."email";

  

-- Delete memberships first (due to foreign key constraints)

DELETE FROM "memberships"

WHERE "memberId" IN (

SELECT "memberId"

FROM "members"

WHERE "email" IN ('trangiahon92@gmail.com', 'hontg+1@vitalify.asia', 'hommatest@gmail.com')

);

  

-- Delete the members (only after memberships are deleted)

DELETE FROM "members"

WHERE "email" IN ('trangiahon92@gmail.com', 'hontg+1@vitalify.asia', 'hommatest@gmail.com');

  

-- If verification fails, you can rollback with ROLLBACK;

-- If everything looks good, commit with:

--COMMIT;
```

#### Delete tenantDB (connection, and tenant owner ) 
```SQL
-- SQL Script to delete tenant connections and tenant owners by tenantOwnerId
-- Replace 'YOUR_TENANT_OWNER_ID_HERE' with the actual tenantOwnerId
-- This script uses transactions to ensure data integrity

START TRANSACTION;

-- Step 1: Delete tenant connections first (they reference tenant owners)
DELETE FROM "tenant_connections" 
WHERE "tenantOwnerId" = '4a099ed5-0e70-4f5d-b622-870f2cb18402';

-- Step 2: Delete tenant owner
DELETE FROM "tenant_owners" 
WHERE "tenantOwnerId" = '4a099ed5-0e70-4f5d-b622-870f2cb18402';

-- Step 3: Verify deletions
SELECT 
    'COMPLETION CHECK' as status,
    (SELECT COUNT(*) FROM "tenant_owners" WHERE "tenantOwnerId" = '4a099ed5-0e70-4f5d-b622-870f2cb18402') as remaining_tenant_owner,
    (SELECT COUNT(*) FROM "tenant_connections" WHERE "tenantOwnerId" = '4a099ed5-0e70-4f5d-b622-870f2cb18402') as remaining_tenant_connections;

-- If verification shows non-zero counts, run ROLLBACK;
-- If everything shows zeros, commit the transaction:
-- COMMIT;
```

#### Drop schema DB
```SQL
-- SQL Script to delete tenant connections, tenant owners AND drop schema by tenantOwnerId

-- Replace 'YOUR_TENANT_OWNER_ID_HERE' with the actual tenantOwnerId

-- Replace 'SCHEMA_NAME_HERE' with the actual schema name

-- This script uses transactions to ensure data integrity

  

START TRANSACTION;

  
DROP SCHEMA IF EXISTS "SCHEMA_NAME_HERE" CASCADE;

  
(SELECT COUNT(*) FROM information_schema.schemata WHERE schema_name = 'SCHEMA_NAME_HERE') as remaining_schema;

-- If verification shows non-zero counts, run ROLLBACK;

-- If everything shows zeros, commit the transaction:

--COMMIT;
```
####
```SQL
START TRANSACTION;

  

DELETE FROM "tenant_signup_requests"

WHERE "email" = 'hommatest@gmail.com';

  

--COMMIT;
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



