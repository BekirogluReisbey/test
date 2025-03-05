### Job Description: Development of a Web App (Login & Dashboard)

#### 1. Technical Requirements

**Frontend Development**
- **Framework:** React with Next.js 13+ for server-side rendering (SSR) and better SEO optimization
- **UI Components:** Material-UI (MUI) v5+ for a consistent design
- **Styling:** Tailwind CSS v3+ for custom, scalable design
- **Responsive Design:** Optimized display on all devices (mobile, tablet, desktop)
- **Programming Language:** TypeScript for improved type safety
- **State Management:** React Context API or Zustand for global states
- **Routing:** Next.js App Router for dynamic route management

---

**Security Measures**

1. **Authentication**
   - JWT Authentication (Access & Refresh Tokens)
   - Secure token storage with HTTP-only cookies
   - Refresh Token Rotation to prevent token theft
   - OTP Verification (One-Time Password) via email during registration & password reset
   - Password Requirements:
     - Minimum length of 8 characters
     - At least one uppercase letter, one number, and one special character
     - Hashing with bcrypt

2. **Authorization**
   - RBAC (Role-Based Access Control) with specific permissions
   - Middleware for protected API routes
   - Dynamic UI permissions based on the user's role
   - Automatic logout after inactivity or session timeout

---

**Backend Architecture**
- **Server:** Node.js with Express.js v4+ or NestJS v9+ for structured API development
- **Programming Language:** TypeScript for better maintainability & type safety
- **API:** RESTful endpoints (alternatively GraphQL with Apollo, if desired)

**Security Measures**
- Helmet.js for secure HTTP headers
- Rate Limiting with express-rate-limit (e.g., max. 100 requests per minute)
- CORS configuration for controlled API access
- CSRF protection for secure form submission
- SQL Injection prevention through prepared statements & ORM security features
- Request validation with Joi or class-validator for robust API validations
- Password hashing with bcrypt (recommended: bcrypt with salt)

---

**Database Design**
- **Database:** MySQL with Sequelize ORM
- **Schema Design:**
  - User management: Supervisor, Admin, Company, Employee, Guest
  - Role & permission system
  - Company-related data (company information, user assignment)
- **Optimizations:**
  - Automatic database migrations for schema changes
  - Indexing for better performance
  - Connection pooling for improved scalability
- **Database Setup:**
  - If not existing, automatic creation of tables
  - If existing, migration of existing database tables
  - Test data generation for developers & testing purposes

---

#### 2. DevOps & Deployment
- **Configuration Management:** via .env files (e.g., for API keys, database access)
- **Deployment:** on a cloud platform
- **CI/CD Pipeline:** for automated tests & deployments

**Expected Documentation**
- Detailed source code with comments
- API documentation (Swagger/OpenAPI)
- Database schema documentation
- Security guide for implementation
- Deployment guide including required packages & environment variables

---

#### 3. Role-Based Access Control & Features

**Frontend & Dashboard Features**

1. **Common Authentication**
   - Login page (/login)
   - Fields: Email, password
   - "Forgot password?" function with email verification
   - No public signup: Users are managed by the supervisor

---

2. **Supervisor Role**
   - Dashboard (/supervisor/dashboard)
   - Overview of all registered companies
   - Create, edit, activate/deactivate companies
   - Company registration (/supervisor/create-company)
   - Input: Company name, contact person, email
   - Automatic invitation for the first admin to set up a password

---

3. **Company Role**
   - Dashboard (/company/dashboard)
   - Overview of company data & employee management
   - Employee management (/company/employees)
   - Add, edit, deactivate employees
   - Access only to own company data
   - Company profile (/company/profile)
   - Edit company details

---

4. **Employee Role**
   - Personal dashboard (/employee/dashboard)
   - Display of own data
   - Change password
   - Profile management (/employee/profile)
   - No access to or editing of other employees or company data

---

5. **Admin Role (Optional, if necessary)**
   - Can manage multiple companies
   - Advanced reporting functions & view logs

---

#### 4. Summary of Roles & Permissions
- **Registration:** Only the supervisor can create companies
- **Companies:** Manage their own data & employees
- **Employees:** Can only edit their own data
- **No user can view or manage another company

---

### Structured List of All Necessary Frontend Pages for Your Web App Based on Defined Roles and Functions:

---

#### 1. Authentication & User Management

✅ /login → Login page
   - Fields: Email, password
   - "Forgot password?" with email input
   - No public registration

✅ /reset-password → Reset password
   - Input of a new password after OTP verification

✅ /2fa-verification → Two-factor authentication (OTP)
   - Input of the OTP code after login

✅ /invite-setup → Invitation for first login
   - Setup page for invited users to create a password

---

#### 2. Supervisor Dashboard (Admin Area)

✅ /supervisor/dashboard → Overview & statistics
   - Number of companies, active users, logs
   - Recent activities

✅ /supervisor/companies → Company management
   - List of all registered companies
   - Create, edit, activate/deactivate companies

✅ /supervisor/create-company → Create new company
   - Fields: Name, contact person, email
   - Automatic invitation of the admin via email

✅ /supervisor/users → User management
   - List of all users (all companies)
   - Block, delete, or change user permissions

✅ /supervisor/audit-logs → Security logs
   - Monitor user activities (e.g., logins, changes)

✅ /supervisor/settings → Global settings
   - Password rules, security measures, API keys

---

#### 3. Company Admin (Company Management)

✅ /company/dashboard → Company overview
   - Number of employees, company profile, recent activities

✅ /company/employees → Employee management
   - List of all employees of the company
   - Add, edit, deactivate

✅ /company/create-employee → Add employee
   - Fields: First name, last name, email, role
   - Automatic invitation to set up a password

✅ /company/profile → Company profile
   - Edit company name, address, contact person

✅ /company/settings → Company settings
   - Security policies, permissions for roles

---

#### 4. Employee Dashboard

✅ /employee/dashboard → Personal homepage
   - Personal tasks, messages, notifications

✅ /employee/profile → Profile management
   - Change name, email, password

✅ /employee/security → Security settings
   - Enable/disable 2FA

✅ /employee/logs → View own login logs

---

#### 5. Error & System Pages

✅ /404 → Page not found
✅ /403 → Access denied
✅ /500 → Internal server error

---

### Summary

🔹 Auth: Login, password reset, OTP
🔹 Supervisor: Admin panel, company & user management, logs
🔹 Company: Dashboard, employee management
🔹 Employee: Own profile, security settings
🔹 System: Error pages

---

### Necessary SQL Database Tables for Your Project with Key Columns, Relationships, and Constraints:

---

#### 1. User Management (users)

This table stores all users (supervisor, companies, employees).

```sql
CREATE TABLE users (
    id              BIGINT AUTO_INCREMENT PRIMARY KEY,
    first_name      VARCHAR(100) NOT NULL,
    last_name       VARCHAR(100) NOT NULL,
    email           VARCHAR(255) UNIQUE NOT NULL,
    password_hash   VARCHAR(255) NOT NULL,
    role_id         BIGINT NOT NULL,
    company_id      BIGINT NULL, -- NULL for supervisors
    is_active       BOOLEAN DEFAULT TRUE,
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    FOREIGN KEY (role_id) REFERENCES roles(id) ON DELETE RESTRICT,
    FOREIGN KEY (company_id) REFERENCES companies(id) ON DELETE SET NULL
);
```

---

#### 2. Roles & Permissions (roles, permissions, role_permissions)

These tables enable role-based access control (RBAC).

**Roles (roles)**

```sql
CREATE TABLE roles (
    id          BIGINT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(50) UNIQUE NOT NULL COMMENT 'e.g., Supervisor, Admin, Company, Employee, Guest',
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Permissions (permissions)**

```sql
CREATE TABLE permissions (
    id          BIGINT AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(100) UNIQUE NOT NULL COMMENT 'e.g., manage_users, view_dashboard',
    created_at  TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

**Assign Roles & Permissions (role_permissions)**

```sql
CREATE TABLE role_permissions (
    role_id        BIGINT NOT NULL,
    permission_id  BIGINT NOT NULL,
    PRIMARY KEY (role_id, permission_id),
    FOREIGN KEY (role_id) REFERENCES roles(id) ON DELETE CASCADE,
    FOREIGN KEY (permission_id) REFERENCES permissions(id) ON DELETE CASCADE
);
```

---

#### 3. Companies (companies)

This table stores companies and is linked to users.

```sql
CREATE TABLE companies (
    id              BIGINT AUTO_INCREMENT PRIMARY KEY,
    name            VARCHAR(255) UNIQUE NOT NULL,
    contact_person  VARCHAR(255) NOT NULL,
    email           VARCHAR(255) UNIQUE NOT NULL,
    is_active       BOOLEAN DEFAULT TRUE,
    created_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at      TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);
```

---

#### 4. Sessions & Authentication (user_sessions, password_resets, otp_verifications)

These tables ensure secure authentication and session management.

**User Sessions (user_sessions)**

```sql
CREATE TABLE user_sessions (
    id            BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id       BIGINT NOT NULL,
    refresh_token VARCHAR(255) UNIQUE NOT NULL,
    user_agent    VARCHAR(255) NULL,
    ip_address    VARCHAR(45) NULL,
    expires_at    TIMESTAMP NOT NULL,
    created_at    TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

**Password Reset Tokens (password_resets)**

```sql
CREATE TABLE password_resets (
    id         BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id    BIGINT NOT NULL,
    token      VARCHAR(255) UNIQUE NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

**OTP Verification (otp_verifications)**

```sql
CREATE TABLE otp_verifications (
    id         BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id    BIGINT NOT NULL,
    otp_code   VARCHAR(10) NOT NULL,
    expires_at TIMESTAMP NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
);
```

---

#### 5. Logs & Security (audit_logs, failed_logins)

For security and monitoring purposes.

**Audit Logs (audit_logs)**

```sql
CREATE TABLE audit_logs (
    id         BIGINT AUTO_INCREMENT PRIMARY KEY,
    user_id    BIGINT NULL,
    action     VARCHAR(255) NOT NULL,
    ip_address VARCHAR(45) NULL,
    user_agent VARCHAR(255) NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE SET NULL
);
```

**Failed Logins (failed_logins)**

```sql
CREATE TABLE failed_logins (
    id         BIGINT AUTO_INCREMENT PRIMARY KEY,
    email      VARCHAR(255) NOT NULL,
    ip_address VARCHAR(45) NULL,
    user_agent VARCHAR(255) NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

---

### Summary of Key Relationships
1. Each user belongs to exactly one role. (users.role_id → roles.id)
2. Supervisors have no company, but companies & employees do.
3. Companies can have multiple users, but a user belongs to only one company.
4. Roles have various permissions via role_permissions.
5. Session management, OTPs, and password resets are securely integrated.

---

### Directory Structure for the Database in React (Next.js)

```
/backend
 ├── src
 │   ├── models  # Here lie the Sequelize models
 │   │   ├── User.ts
 │   │   ├── Role.ts
 │   │   ├── Permission.ts
 │   │   ├── Company.ts
 │   │   ├── Session.ts
 │   │   ├── AuditLog.ts
 │   │   ├── index.ts  # Links all models
 │   ├── config
 │   │   ├── database.ts  # Database connection
 │   ├── migrations
 │   │   ├── 20240306-create-tables.ts  # Automatic migrations
```

---

#### 1. Sequelize Database Connection (database.ts)

This file connects your backend to the MySQL database.

```typescript
import { Sequelize } from 'sequelize';

const sequelize = new Sequelize(process.env.DB_NAME || 'mydb', process.env.DB_USER || 'root', process.env.DB_PASSWORD || '', {
    host: process.env.DB_HOST || 'localhost',
    dialect: 'mysql',
    logging: false, // Disable SQL logging for clean logs
    pool: { max: 10, min: 0, acquire: 30000, idle: 10000 }, // Connection pooling
});

export default sequelize;
```

---

#### 2. Database Models (models/)

Here are all tables as Sequelize models in TypeScript:

**2.1 User (User.ts)**

```typescript
import { DataTypes, Model } from 'sequelize';
import sequelize from '../config/database';
import Role from './Role';
import Company from './Company';

class User extends Model {}

User.init({
    id: { type: DataTypes.BIGINT, autoIncrement: true, primaryKey: true },
    firstName: { type: DataTypes.STRING(100), allowNull: false },
    lastName: { type: DataTypes.STRING(100), allowNull: false },
    email: { type: DataTypes.STRING(255), allowNull: false, unique: true },
    passwordHash: { type: DataTypes.STRING(255), allowNull: false },
    isActive: { type: DataTypes.BOOLEAN, defaultValue: true },
}, { sequelize, modelName: 'user', timestamps: true });

User.belongsTo(Role, { foreignKey: 'roleId', onDelete: 'RESTRICT' });
User.belongsTo(Company, { foreignKey: 'companyId', onDelete: 'SET NULL' });

export default User;
```

---

**2.2 Roles (Role.ts)**

```typescript
import { DataTypes, Model } from 'sequelize';
import sequelize from '../config/database';

class Role extends Model {}

Role.init({
    id: { type: DataTypes.BIGINT, autoIncrement: true, primaryKey: true },
    name: { type: DataTypes.STRING(50), allowNull: false, unique: true },
}, { sequelize, modelName: 'role', timestamps: true });

export default Role;
```

---

**2.3 Permissions (Permission.ts)**

```typescript
import { DataTypes, Model } from 'sequelize';
import sequelize from '../config/database';

class Permission extends Model {}

Permission.init({
    id: { type: DataTypes.BIGINT, autoIncrement: true, primaryKey: true },
    name: { type: DataTypes.STRING(100), allowNull: false, unique: true },
}, { sequelize, modelName: 'permission', timestamps: true });

export default Permission;
```

---

**2.4 Companies (Company.ts)**

```typescript
import { DataTypes, Model } from 'sequelize';
import sequelize from '../config/database';

class Company extends Model {}

Company.init({
    id: { type: DataTypes.BIGINT, autoIncrement: true, primaryKey: true },
    name: { type: DataTypes.STRING(255), allowNull: false, unique: true },
    contactPerson: { type: DataTypes.STRING(255), allowNull: false },
    email: { type: DataTypes.STRING(255), allowNull: false, unique: true },
    isActive: { type: DataTypes.BOOLEAN, defaultValue: true },
}, { sequelize, modelName: 'company', timestamps: true });

export default Company;
```

---

**2.5 User Sessions (Session.ts)**

```typescript
import { DataTypes, Model } from 'sequelize';
import sequelize from '../config/database';
import User from './User';

class Session extends Model {}

Session.init({
    id: { type: DataTypes.BIGINT, autoIncrement: true, primaryKey: true },
    refreshToken: { type: DataTypes.STRING(255), allowNull: false, unique: true },
    userAgent: { type: DataTypes.STRING(255) },
    ipAddress: { type: DataTypes.STRING(45) },
    expiresAt: { type: DataTypes.DATE, allowNull: false },
}, { sequelize, modelName: 'session', timestamps: true });

Session.belongsTo(User, { foreignKey: 'userId', onDelete: 'CASCADE' });

export default Session;
```

---

**2.6 Audit Logs (AuditLog.ts)**

```typescript
import { DataTypes, Model } from 'sequelize';
import sequelize from '../config/database';
import User from './User';

class AuditLog extends Model {}

AuditLog.init({
    id: { type: DataTypes.BIGINT, autoIncrement: true, primaryKey: true },
    action: { type: DataTypes.STRING(255), allowNull: false },
    ipAddress: { type: DataTypes.STRING(45) },
    userAgent: { type: DataTypes.STRING(255) },
}, { sequelize, modelName: 'audit_log', timestamps: true });

AuditLog.belongsTo(User, { foreignKey: 'userId', onDelete: 'SET NULL' });

export default AuditLog;
```

---

#### 3. Import All Models in models/index.ts

```typescript
import sequelize from '../config/database';
import User from './User';
import Role from './Role';
import Permission from './Permission';
import Company from './Company';
import Session from './Session';
import AuditLog from './AuditLog';

const models = { User, Role, Permission, Company, Session, AuditLog };

export default models;
export { sequelize };
```

---

#### 4. Migrations (Automatic Table Creation)

Create in backend/src/migrations/20240306-create-tables.ts:

```typescript
import { sequelize } from '../models';

async function migrate() {
    try {
        await sequelize.sync({ alter: true });
        console.log('✅ Database tables successfully created or updated.');
    } catch (error) {
        console.error('❌ Error creating tables:', error);
    }
}

migrate();
```

---

#### 5. Setup & Start Migration
1. Install Sequelize & MySQL Driver:

```bash
npm install sequelize mysql2
```

2. Configure environment (.env):

```
DB_HOST=localhost
DB_USER=root
DB_PASSWORD=
DB_NAME=mydb
```

3. Run migration:

```bash
node backend/src/migrations/20240306-create-tables.js
```
