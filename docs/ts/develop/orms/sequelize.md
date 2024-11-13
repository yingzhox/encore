---
seotitle: Using Sequelize with Encore.ts
seodesc: Learn how to use Sequelize with Encore to interact with SQL databases.
title: Using Sequelize with Encore.ts
lang: ts
---
Encore.ts supports integrating [Sequelize](https://sequelize.org/), a promise-based Node.js ORM. To set up Sequelize with Encore, start by creating a `SQLDatabase` instance and providing the connection string to Sequelize.

## 1. Setting Up the Database Connection

In `database.ts`, initialize the `SQLDatabase` and configure Sequelize:

```typescript
// database.ts
import {
  Model,
  InferAttributes,
  InferCreationAttributes,
  CreationOptional,
  DataTypes,
  Sequelize,
} from "sequelize";
import { SQLDatabase } from "encore.dev/storage/sqldb";

// Create SQLDatabase instance with migrations configuration
const DB = new SQLDatabase("encore_sequelize_test", {
  migrations: "./migrations",
});

// Initialize Sequelize with the connection string
const sequelize = new Sequelize(DB.connectionString);

// Define the User model
class User extends Model<InferAttributes<User>, InferCreationAttributes<User>> {
  declare id: CreationOptional<number>;
  declare name: string;
  declare surname: string;
}

// Example usage: Count all users
const count = await User.count();
```

## 2. Creating Migrations

Encore does not currently support JavaScript migration files generated by tools like `sequelize-cli model:generate`. Instead, create and manage your own [migration files](/docs/ts/primitives/databases#database-migrations) in SQL format.

Example migration file for creating the `user` table:

```sql
-- migrations/1_create_user.up.sql --
CREATE TABLE "user" (
  id SERIAL PRIMARY KEY,
  name TEXT NOT NULL,
  surname TEXT NOT NULL
);
```

## 3. Applying Migrations

Migrations are automatically applied when you run your Encore application, so you don’t need to run `sequelize db:migrate` or similar commands manually.

--- 

For more information, see the example on GitHub:  
<GitHubLink href="https://github.com/encoredev/examples/tree/main/ts/sequelize" desc="Using Sequelize ORM with Encore.ts" />