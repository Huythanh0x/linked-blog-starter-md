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
#### Tenant Owner

| Column Name       | Data Type    | Constraints      |
| ----------------- | ------------ | ---------------- |
| **tenantOwnerId** | uuid         | PK, Generated    |
| ownerId           | varchar(255) | PK               |
| name              | varchar(255) | NOT NULL         |
| email             | varchar(255) | NOT NULL, UNIQUE |
| password          | varchar(255) | NOT NULL         |
| avatar            | varchar(500) | NULLABLE         |
| status            | smallint     | NOT NULL         |
| lastLogin         | timestamp    | NULLABLE         |
| createdAt         | datetime     | NULLABLE         |
| updatedAt         | datetime     | NULLABLE         |
| deletedAt         | datetime     | NULLABLE         |

**Notes:**
- Both entities extend `AbstractEntity` which likely provides the timestamp fields (createdAt, updatedAt, deletedAt)
- `tenant_owners` is in the root schema while `owners` is in the tenant schema
- The main differences are:
  - Primary key generation (uuid vs varchar)
  - tenant_owners focuses on name while owners focuses on authentication (email/password)
  - owners has additional fields for authentication tracking (lastLogin)

Would you like me to add any additional details or organize it differently?