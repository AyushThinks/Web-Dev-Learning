# Day 18 — Storing Data, Step by Step
### Sequelize + MySQL

---

## 📖 Overview

This session covers **Sequelize**, an ORM (Object Relational Mapper) that lets you talk to MySQL using JavaScript instead of writing raw SQL. It walks through connecting to a database, defining models, setting up relationships, and performing full CRUD operations — plus how to securely hash passwords with bcrypt.

---

## 🔹 What is Sequelize?

Sequelize lets you write JavaScript instead of SQL to interact with your database.

**Raw SQL:**
```sql
INSERT INTO Users (name, email, password) VALUES ('Ayush', 'ayushthinks@email.com', '123');
```

**Sequelize equivalent:**
```javascript
await User.create({ name: "Ayush", email: "ayushthinks@email.com", password: "123" });
```

Same result — but in JavaScript.

---

## 🔧 Setup

### `configurations/database.js` — MySQL Connection

```javascript
const { Sequelize } = require("sequelize");

const sequelize = new Sequelize(
  "resume_db", // database name (create this in MySQL Workbench first)
  "root", // MySQL username
  "your_password", // MySQL password
  {
    host: "localhost",
    dialect: "mysql",
    logging: false, // set to console.log to see every SQL query
  },
);

module.exports = sequelize;
```

---

## 🗂️ Models

### `models/user.js` — Users Table

```javascript
const { DataTypes } = require("sequelize");
const bcrypt = require("bcryptjs");
const sequelize = require("../config/database");

const User = sequelize.define("User", {
  name: { type: DataTypes.STRING, allowNull: false },
  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true,
    validate: { isEmail: true },
  },
  password: { type: DataTypes.STRING, allowNull: false },
});

// Hash password before saving to database
User.beforeCreate(async (user) => {
  const salt = await bcrypt.genSalt(10);
  user.password = await bcrypt.hash(user.password, salt);
});

// Method to check password on login
User.prototype.checkPassword = function (plainText) {
  return bcrypt.compare(plainText, this.password);
};

// Relationship — one user has many resumes
User.associate = (models) => {
  User.hasMany(models.Resume, { foreignKey: "userId", onDelete: "CASCADE" });
};

module.exports = User;
```

### `models/resume.js` — Resumes Table

```javascript
const { DataTypes } = require("sequelize");
const sequelize = require("../config/database");

const Resume = sequelize.define("Resume", {
  title: { type: DataTypes.STRING, allowNull: false },
  summary: { type: DataTypes.TEXT },
});

// Relationship — resume belongs to a user
Resume.associate = (models) => {
  Resume.belongsTo(models.User, { foreignKey: "userId" });
};

module.exports = Resume;
```

### `models/index.js` — Load All Models + Wire Associations

```javascript
const sequelize = require("../config/database");
const User = require("./user");
const Resume = require("./resume");

const models = { User, Resume };

// Wire up all associations after all models are loaded
Object.values(models).forEach((model) => {
  if (model.associate) model.associate(models);
});

module.exports = { sequelize, ...models };
```

---

## ⚙️ Operations — Step by Step

### Step 1: Sync Database (Create Tables)

```javascript
const { sequelize } = require("./models");
await sequelize.sync(); // creates tables if they don't exist
```

> `sync()` without `force: true` keeps existing data safe.
> `sync({ force: true })` drops and recreates all tables — use only in development.

---

### Step 2: Create a User

```javascript
const user = await User.create({
  name: "Ayush",
  email: "ayushthinks@example.com",
  password: "secret123",
});
console.log("Saved user #" + user.id);
```

**SQL Sequelize runs:**
```sql
INSERT INTO `Users` (`name`, `email`, `password`, `createdAt`, `updatedAt`)
VALUES ('Ayush', 'ayushthinks@example.com', ..., NOW(), NOW());
```

> `createdAt` and `updatedAt` are added automatically by Sequelize.
> `user.id` is the auto-generated primary key.

---

### Step 3: Create Resumes for That User

```javascript
await Resume.create({
  title: "Full Stack Intern",
  summary: "Built REST APIs with Node, Express and MySQL.",
  userId: user.id,
});
```

> `userId: user.id` links the resume to the user (foreign key).

---

### Step 4: Read All Resumes of a User

```javascript
const resumes = await Resume.findAll({ where: { userId: user.id } });
resumes.forEach((r) => console.log(r.title));
```

**SQL Sequelize runs:**
```sql
SELECT * FROM `Resumes` WHERE `userId` = 1;
```

---

### Step 5: Read One Resume With Its User (JOIN)

```javascript
const resume = await Resume.findByPk(1, { include: User });
console.log(resume.title, "belongs to", resume.User.name);
```

**SQL Sequelize runs:**
```sql
SELECT Resume.*, User.name, User.email
FROM `Resumes` AS `Resume`
LEFT OUTER JOIN `Users` AS `User`
ON `Resume`.`userId` = `User`.`id`
WHERE `Resume`.`id` = 1;
```

> `include: User` tells Sequelize to JOIN the Users table automatically.

---

### Step 6: Update a Resume

```javascript
resume.title = "Senior Full Stack Intern";
await resume.save();
```

**SQL Sequelize runs:**
```sql
UPDATE `Resumes` SET `title` = 'Senior Full Stack Intern', `updatedAt` = NOW()
WHERE `id` = 1;
```

> `.save()` only updates changed fields — `updatedAt` is updated automatically.

---

## 🔑 Key Concepts

| Concept | What It Does |
|---|---|
| `sequelize.define()` | Creates a model (maps to a database table) |
| `sequelize.sync()` | Creates tables in MySQL if they don't exist |
| `Model.create()` | INSERT — adds a new row |
| `Model.findAll()` | SELECT — gets multiple rows |
| `Model.findByPk()` | SELECT — gets one row by primary key |
| `Model.findOne()` | SELECT — gets first matching row |
| `instance.save()` | UPDATE — saves changes to existing row |
| `instance.destroy()` | DELETE — removes a row |
| `include:` | JOIN — fetches related model data |
| `where:` | WHERE clause — filters results |
| `beforeCreate` | Hook — runs before INSERT (used for hashing password) |
| `hasMany` | One-to-many relationship |
| `belongsTo` | Many-to-one relationship |
| `onDelete: CASCADE` | Deletes related rows when parent is deleted |

---

## 🔒 bcrypt — Password Hashing

Never store plain text passwords. bcrypt hashes them before saving, and compares them safely at login.

```javascript
const bcrypt = require("bcryptjs");

// Hash password
const salt = await bcrypt.genSalt(10);
const hashed = await bcrypt.hash("secret123", salt);

// Check password on login
const isMatch = await bcrypt.compare("secret123", hashed); // true
```

> `genSalt(10)` — 10 rounds of hashing (higher = more secure but slower).