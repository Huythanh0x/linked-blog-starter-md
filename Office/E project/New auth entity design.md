### Tenant Connection (Tenants)

| Column Name          | Data Type    | Constraints              |
| -------------------- | ------------ | ------------------------ |
| *tenantConnectionId* | uuid         | PK, Generated            |
| schemaName           | varchar(255) |                          |
| schemaPassword       | varchar(255) |                          |
| userName             | varchar(255) |                          |
| userPassword         | varchar(255) |                          |
| **tenantOwnerId**    | uuid         | 1:1 with **TenantOwner** |

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
Connect between member<mark style="background: #BBFABBA6;">(user, owner)</mark> and <mark style="background: #FFB86CA6;">(tenant)</mark>

| Column Name       | Data Type | Constraints                      |
| ----------------- | --------- | -------------------------------- |
| *membershipId*    | uuid      | PK, Generated                    |
| **tenantOwnerId** | uuid      | Link to ***Tennant Connection*** |
| **userId**        | uuid      | Link to ***User***               |
| role              | smallint  | MEMBER or TENANT_ADMIN           |


### Update tenant schema
- Remove `owners` -> move to `members`
- Create mapping **Email-TenantOwnerId** -> `memberships`
- Link `members` to `users` so that after login with member creds -> can return `users` indentity for later query
### Update the flow
#### Login
##### Tenant Login
return `tenantOw`
##### User Login