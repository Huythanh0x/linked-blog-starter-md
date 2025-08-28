| Column Name      | Data Type    | Constraints      | Default | Description                                                            |
| ---------------- | ------------ | ---------------- | ------- | ---------------------------------------------------------------------- |
| userId           | uuid         | PK, Generated    | -       | Primary key                                                            |
| email            | varchar(255) | NOT NULL, UNIQUE | -       | User's email address                                                   |
| password         | varchar(255) | NULLABLE         | -       | User's password                                                        |
| stripeCustomerId | varchar(255) | NULLABLE         | -       | Stripe customer ID                                                     |
| name             | varchar(255) | NULLABLE         | -       | User's name                                                            |
| avatar           | varchar      | NULLABLE         | -       | User's avatar URL                                                      |
| status           | smallint     | NOT NULL         | 1       | Status code:<br>1: ACTIVE<br>0: INACTIVE<br>2: SUSPENDED<br>3: DELETED |
| feedBackCount    | smallint     | NOT NULL         | 0       | Number of feedbacks                                                    |
| lastLogin        | timestamp    | NULLABLE         | -       | Last login timestamp                                                   |
| isVerified       | smallint     | NOT NULL         | 0       | Verification status:<br>0: Not verified<br>1: Verified                 |
