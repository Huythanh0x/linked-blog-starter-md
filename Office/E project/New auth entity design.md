### Tenant Connection (Tenants)

| Column Name          | Data Type    |
| -------------------- | ------------ |
| *tenantConnectionId* | uuid         |
| schemaName           | varchar(255) |
| schemaPassword       | varchar(255) |
| userName             | varchar(255) |
| userPassword         | varchar(255) |

### User

| Column Name      | Data Type    | Constraints      |
| ---------------- | ------------ | ---------------- |
| *userId*         | uuid         | PK, Generated    |
| email            | varchar(255) | NOT NULL, UNIQUE |
| password         | varchar(255) | NULLABLE         |
| stripeCustomerId | varchar(255) | NULLABLE         |
| name             | varchar(255) | NULLABLE         |
| status           | smallint     | NOT NULL         |
| lastLogin        | timestamp    | NULLABLE         |
| isVerified       | smallint     | NOT NULL         |
### Tenant Owner (Owner)

| Column Name     | Data Type    | Constraints      |
| --------------- | ------------ | ---------------- |
| *tenantOwnerId* | uuid         | PK, Generated    |
| email           | varchar(255) | NOT NULL, UNIQUE |
| name            | varchar(255) | NOT NULL         |
| password        | varchar(255) | NOT NULL         |

### Memberships

| Column Name       | Data Type | Constraints            |
| ----------------- | --------- | ---------------------- |
| *membershipId*    | uuid      |                        |
| **tenantOwnerId** | uuid      |                        |
| **userId**        | uuid      |                        |
| role              | smallint  | MEMBER or TENANT_ADMIN |


