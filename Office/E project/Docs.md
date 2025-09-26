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

### Query tenantowner DB by email
```SQL
SELECT tenant_owners.*
	FROM tenant_owners
	JOIN memberships ON memberships."tenantOwnerId" = tenant_owners."tenantOwnerId"
	JOIN members ON members."memberId" = memberships."memberId"
	WHERE members.email IN ('hommatest@gmail.com');
```

### Delete user/tenant (member,  membership, users and related data to users)
#### Delete member and memberships (tenant and user)
```SQL
START TRANSACTION;
-- Delete membersips
DELETE FROM "memberships"
WHERE "memberId" IN (
    SELECT "memberId"
    FROM "members"
    WHERE "email" IN ('hommatest@gmail.com')
);

-- Delete members
DELETE FROM "members"
WHERE "email" IN ('hommatest@gmail.com');
COMMIT;
```

#### Tenant Sign up
```SQL
START TRANSACTION;

DELETE FROM "tenant_signup_requests"
WHERE "email" IN ('hommatest@gmail.com');

COMMIT;
```
#### Delete tenantDB (connection, and tenant owner ) 
```SQL
START TRANSACTION;

-- Step 1: Delete tenant connections first (they reference tenant owners)
DELETE FROM "tenant_connections" 
WHERE "tenantOwnerId" = '0c6144ce-ab0b-4182-9e78-8e251309dff6';

-- Step 2: Delete tenant owner
DELETE FROM "tenant_owners" 
WHERE "tenantOwnerId" = '0c6144ce-ab0b-4182-9e78-8e251309dff6';

COMMIT;
```

#### Drop schema DB
```SQL
DROP SCHEMA IF EXISTS "hommatest_gmail_com_6bdd739174" CASCADE;
```

#### Delete postgres user
```SQL
--DROP OWNED BY user_hommatest_gmail_com_6bdd739174;
DROP USER user_hommatest_gmail_com_6bdd739174;

```
### Drop tenant data completely (all in one)
```SQL
-- ==============================================================================

-- AUTOMATED TENANT DELETION SCRIPT

-- ==============================================================================

-- REQS: Takes an email input only and systematically removes:

-- - Database schema & database user

-- - All related members and memberships

-- - Tenant connections

-- - Tenant owner and requests

-- ==============================================================================

  

-- STEP 1: Edit this line and change the email to delete

-- \set_target_email 'hommatest@gmail.com'

-- Then run the entire script

  

-- ========== ANONYMUS BLOCK TO DISCOVER ALL RELATIONSHIPS ==========

DO $$

DECLARE

search_email constant text := 'hommatest@gmail.com'; -- EDIT THIS EMAIL ONLY

found_tenant_owner_id uuid;

discovered_schema text;

discovered_db_user text;

all_tenant_user_emails text[] := '{}';

all_members_to_delete text[];

tenant_email_item text;

process_start time := now();

-- Counters for final report

memberships_deleted int := 0;

members_deleted int := 0;

connections_deleted int := 0;

owners_deleted int := 0;

schemas_dropped int := 0;

users_dropped int := 0;

BEGIN

RAISE NOTICE '==== Starting complete tenant deletion for: % ====', search_email;

  

-- ==================== STEP 1: DISCOVER TENANT STRUCTURE ====================

RAISE NOTICE 'Step 1: Discovering tenant structure...';

-- Get the tenant owner ID from email

SELECT

to_owner."tenantOwnerId"

INTO found_tenant_owner_id

FROM "tenant_owners" to_owner

JOIN "memberships" ms ON ms."tenantOwnerId" = to_owner."tenantOwnerId"

JOIN "members" m ON ms."memberId" = m."memberId"

WHERE m."email" = search_email;

IF found_tenant_owner_id IS NULL THEN

RAISE NOTICE '❌ No tenant owner found for email: %. Exiting...', search_email;

RETURN;

END IF;

RAISE NOTICE '✅ Found tenant owner: %', found_tenant_owner_id;

-- Get schema and database user info

SELECT

tc."schemaName",

tc."userName"

INTO discovered_schema, discovered_db_user

FROM "tenant_connections" tc

WHERE tc."tenantOwnerId" = found_tenant_owner_id;

RAISE NOTICE '✅ Schema found: % | DB User: %', discovered_schema, discovered_db_user;

  

-- ==================== STEP 2: DISCOVER ALL USERS ====================

RAISE NOTICE 'Step 2: Discovering all users in tenant database...';

IF discovered_schema IS NOT NULL THEN

BEGIN

-- Get all email addresses from the tenant database users table

FOR tenant_email_item IN

EXECUTE FORMAT('SELECT "email" FROM %I."users"', discovered_schema)

LOOP

all_tenant_user_emails := array_append(all_tenant_user_emails, tenant_email_item);

END LOOP;

RAISE NOTICE '✅ Found % tenant users', array_length(all_tenant_user_emails, 1);

EXCEPTION WHEN OTHERS THEN

RAISE NOTICE '⚠️ Could not access tenant database %. May not exist.', discovered_schema;

END;

END IF;

-- ==================== STEP 3: DELETE DATABASE SCHEMA & USER ====================

RAISE NOTICE 'Step 3: Dropping database schema and user...';

-- Drop the schema from tenant

IF discovered_schema IS NOT NULL THEN

BEGIN

EXECUTE 'DROP SCHEMA IF EXISTS "' || discovered_schema || '" CASCADE';

RAISE NOTICE '✅ Dropped schema: %', discovered_schema;

schemas_dropped := 1;

EXCEPTION WHEN OTHERS THEN

RAISE NOTICE '⚠️ Failed to drop schema %: %', discovered_schema, SQLERRM;

END;

END IF;

-- Drop database user associated to tenant

IF discovered_db_user IS NOT NULL THEN

BEGIN

EXECUTE 'DROP OWNED BY ' || discovered_db_user;

EXECUTE 'DROP USER IF EXISTS ' || discovered_db_user;

RAISE NOTICE '✅ Dropped database user: %', discovered_db_user;

users_dropped := 1;

EXCEPTION WHEN OTHERS THEN

RAISE NOTICE '⚠️ Failed to drop user %: %', discovered_db_user, SQLERRM;

END;

END IF;

  

-- ==================== STEP 4: DELETE ALL MEMBER DATA ====================

RAISE NOTICE 'Step 4: Removing all memberships and members...';

-- Combine original email with any tenant database emails discovered

all_members_to_delete := ARRAY[search_email];

IF array_length(all_tenant_user_emails, 1) > 0 THEN

all_members_to_delete := all_members_to_delete || all_tenant_user_emails;

END IF;

-- Remove duplicates

all_members_to_delete := ARRAY(SELECT DISTINCT unnest(all_members_to_delete));

RAISE NOTICE 'Deleting members for emails: %', all_members_to_delete;

-- Delete memberships first (to remove foreign key constraints)

DELETE FROM "memberships"

WHERE "tenantOwnerId" = found_tenant_owner_id;

GET DIAGNOSTICS memberships_deleted = ROW_COUNT;

RAISE NOTICE '✅ Deleted % memberships', memberships_deleted;

-- Delete all member records

DELETE FROM "members"

WHERE "email" = ANY(all_members_to_delete);

GET DIAGNOSTICS members_deleted = ROW_COUNT;

RAISE NOTICE '✅ Deleted % member records', members_deleted;

  

-- ==================== STEP 5: CLEANUP TENANT OWNER DATA ====================

RAISE NOTICE 'Step 5: Removing tenant owner and signup pre-registration...';

-- Delete signup requests (sometimes verify before deleting)

IF EXISTS (SELECT FROM "tenant_signup_requests" WHERE "email" = search_email) THEN

DELETE FROM "tenant_signup_requests" WHERE "email" = search_email;

RAISE NOTICE '✅ Deleted signup request for %', search_email;

ELSE

RAISE NOTICE 'No signup request found for %', search_email;

END IF;

-- Delete tenant connections one-liner

DELETE FROM "tenant_connections" WHERE "tenantOwnerId" = found_tenant_owner_id;

GET DIAGNOSTICS connections_deleted = ROW_COUNT;

RAISE NOTICE '✅ Deleted % tenant connection ''s', connections_deleted;

-- Delete tenant owner

DELETE FROM "tenant_owners" WHERE "tenantOwnerId" = found_tenant_owner_id;

GET DIAGNOSTICS owners_deleted = ROW_COUNT;

RAISE NOTICE '✅ Deleted % tenant owner records', owners_deleted;

  

-- ==================== STEP 6: VERIFY SUCCESS ====================

RAISE NOTICE 'Step 6: Performing verification...';

-- Check an extra member association remaining

IF EXISTS (

SELECT 1 FROM "members"

WHERE "email" = search_email

) THEN

RAISE NOTICE '⚠️ WARNING: Member/s still exist in email %', search_email;

END IF;

IF EXISTS (

SELECT 1

FROM "tenant_owners" to_check

WHERE to_check."tenantOwnerId" = found_tenant_owner_id

) THEN

RAISE NOTICE '⚠️ WARNING: Tenant owner still exists!';

END IF;

RAISE NOTICE '==== SUCCESS SUMMARY ====';

RAISE NOTICE 'Members shortened: %', members_deleted;

RAISE NOTICE 'Memberships cleared: %', memberships_deleted;

RAISE NOTICE 'Owners removed: %', owners_deleted;

RAISE NOTICE 'Schema dropped : %', schemas_dropped;

RAISE NOTICE 'Database users revoke: %', users_dropped;

RAISE NOTICE 'Total time elapsed: %', NOW() - process_start;

IF (

members_deleted > 0

OR memberships_deleted > 0

OR owners_deleted > 0

OR schemas_dropped > 0

OR users_dropped > 0

) THEN

RAISE NOTICE '🎯 TENANT WIPING COMPLETED SUCCESSFULLY FOR %', search_email;

ELSE

RAISE NOTICE '⚠️ NOTHING TO DELETE OR PROCESS ';

END IF;

  

-- At this point, nothing related to the "grouped tenant" should remain

END $$;

  

-- You can also set ::email_to_delete directly if your database shell supports vars:

SELECT NULL AS status_after_deletion;
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



