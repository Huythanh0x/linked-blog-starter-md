### User

| Column Name      | Data Type    | Constraints      |
| ---------------- | ------------ | ---------------- |
| *userId*         | uuid         | PK, Generated    |
| **email**        | varchar(255) | NOT NULL, UNIQUE |
| password         | varchar(255) | NULLABLE         |
| stripeCustomerId | varchar(255) | NULLABLE         |
| name             | varchar(255) | NULLABLE         |
| status           | smallint     | NOT NULL         |
| lastLogin        | timestamp    | NULLABLE         |
| isVerified       | smallint     | NOT NULL         |
### Tenants

| Column Name       | Data Type    | Constraints   |
| ----------------- | ------------ | ------------- |
| **tenantOwnerId** | uuid         | PK, Generated |
| name              | varchar(255) | NOT NULL      |
| password          | varchar(255) | NOT NULL      |

### Memberships


