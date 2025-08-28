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

| Column Name   | Data Type    | Constraints      | Default    | Source Table  | Description                                                            |
| ------------- | ------------ | ---------------- | ---------- | ------------- | ---------------------------------------------------------------------- |
| tenantOwnerId | uuid         | PK, Generated    | -          | tenant_owners | Primary key for tenant owners                                          |
| ownerId       | varchar(255) | PK               | -          | owners        | Primary key for owners                                                 |
| name          | varchar(255) | NOT NULL         | -          | tenant_owners | Tenant owner's name                                                    |
| email         | varchar(255) | NOT NULL, UNIQUE | -          | owners        | Owner's email address                                                  |
| password      | varchar(255) | NOT NULL         | -          | owners        | Owner's password                                                       |
| avatar        | varchar(500) | NULLABLE         | -          | owners        | Owner's avatar URL                                                     |
| status        | smallint     | NOT NULL         | 1 (ACTIVE) | both          | Status code:<br>1: ACTIVE<br>0: INACTIVE<br>2: SUSPENDED<br>3: DELETED |
| lastLogin     | timestamp    | NULLABLE         | -          | owners        | Last login timestamp                                                   |
| createdAt     | datetime     | NULLABLE         | -          | both*         | Creation timestamp                                                     |
| updatedAt     | datetime     | NULLABLE         | -          | both*         | Last update timestamp                                                  |
| deletedAt     | datetime     | NULLABLE         | -          | both*         | Deletion timestamp                                                     |

**Notes:**
- Both entities extend `AbstractEntity` which likely provides the timestamp fields (createdAt, updatedAt, deletedAt)
- `tenant_owners` is in the root schema while `owners` is in the tenant schema
- The main differences are:
  - Primary key generation (uuid vs varchar)
  - tenant_owners focuses on name while owners focuses on authentication (email/password)
  - owners has additional fields for authentication tracking (lastLogin)

Would you like me to add any additional details or organize it differently?