# OrgLog- A User Authentication & Organisation Management API

## Description

This API provides user authentication and organization management functionality. Users can register, log in, and manage organizations they belong to. Each user belongs to one or more organizations, and each organization can have multiple users.

## Table of Contents

- [Description](#description)
- [Technologies Used](#technologies-used)
- [Setup and Installation](#setup-and-installation)
- [Environment Variables](#environment-variables)
- [Models](#models)
  - [User Model](#user-model)
  - [Organisation Model](#organisation-model)
- [Endpoints](#endpoints)
  - [Authentication](#authentication)
    - [POST /auth/register](#post-authregister)
    - [POST /auth/login](#post-authlogin)
  - [User](#user)
    - [GET /api/users/:id](#get-apiusersid)
  - [Organisation](#organisation)
    - [GET /api/organisations](#get-apiorganisations)
    - [GET /api/organisations/:orgId](#get-apiorganisationsorgid)
    - [POST /api/organisations](#post-apiorganisations)
    - [POST /api/organisations/:orgId/users](#post-apiorganisationsorgidusers)
- [Validation Errors](#validation-errors)
- [Unit Testing](#unit-testing)
  - [End-to-End Test Requirements](#end-to-end-test-requirements)

## Technologies Used

- Node.js
- Nest.js
- PostgreSQL
- TypeORM
- JWT for authentication
- Jest for testing

## Setup and Installation

#### 1. Clone the repository:

```bash
git clone https://github.com/CollinDex/btsk-2
```

#### 2. Install dependencies:
```bash
npm install
```

#### 3. Create a \`.env\` file and add the necessary environment variables (see below).

#### 4. Run the application:
```bash
nest start
```

## Environment Variables

Create a \`.env\` file in the root directory with the following variables:

```bash
PG_HOST=your_host
PG_PORT=5432
PG_USER=your_user
PG_PASSWORD=your_password
PG_DB=your_database
PG_SSLMODE=require
JWT_SECRET=your_jwt_secret
JWT_EXPIRATION=1h
NODE_ENV=development
```

## Models

### User Model

```bash
{
  "userId": "string",
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "password": "string",
  "phone": "string"
}
```

### Organisation Model

```bash
{
  "orgId": "string",
  "name": "string",
  "description": "string"
}
```

## Endpoints

### Authentication

#### POST /auth/register

**Description:** Registers a user and creates a default organisation.

**Request Body:**

```bash
{
  "firstName": "string",
  "lastName": "string",
  "email": "string",
  "password": "string",
  "phone": "string"
}
```

**Successful Response:**

```bash
{
  "status": "success",
  "message": "Registration successful",
  "data": 
  {
    "accessToken": "eyJh...",
    "user": 
    {
      "userId": "string",
      "firstName": "string",
      "lastName": "string",
      "email": "string",
      "phone": "string"
    }
  }
}
```

**Unsuccessful Response:**

```bash
{
  "status": "Bad request",
  "message": "Registration unsuccessful",
  "statusCode": 400
}
```

#### POST /auth/login

**Description:** Logs in a user.

**Request Body:**

```bash
{
  "email": "string",
  "password": "string"
}
```

**Successful Response:**

```bash
{
  "status": "success",
  "message": "Login successful",
  "data": 
  {
    "accessToken": "eyJh...",
    "user": 
    {
      "userId": "string",
      "firstName": "string",
      "lastName": "string",
      "email": "string",
      "phone": "string"
    }
  }
}
```

**Unsuccessful Response:**

```shell
{
  "status": "Bad request",
  "message": "Authentication failed",
  "statusCode": 401
}
```

### User

#### GET /api/users/:id

**Description:** Gets the record of the logged-in user or users in organizations they belong to or created.

**Successful Response:**

```bash
{
  "status": "success",
  "message": "<message>",
  "data": 
  {
    "userId": "string",
    "firstName": "string",
    "lastName": "string",
    "email": "string",
    "phone": "string"
  }
}
```

### Organisation

#### GET /api/organisations

**Description:** Gets all organisations the user belongs to or created.

**Successful Response:**

```bash
{
  "status": "success",
  "message": "<message>",
  "data": 
  {
    "organisations": [
      {
        "orgId": "string",
        "name": "string",
        "description": "string"
      }
    ]
  }
}
```

#### GET /api/organisations/:orgId

**Description:** Gets a single organisation record.

**Successful Response:**

```bash
{
  "status": "success",
  "message": "<message>",
  "data": 
  {
    "orgId": "string",
    "name": "string",
    "description": "string"
  }
}
```

#### POST /api/organisations

**Description:** Creates a new organisation.

**Request Body:**

```bash
{
  "name": "string",
  "description": "string"
}
```

**Successful Response:**

```bash
{
  "status": "success",
  "message": "Organisation created successfully",
  "data": 
  {
    "orgId": "string",
    "name": "string",
    "description": "string"
  }
}
```

**Unsuccessful Response:**

```bash
{
  "status": "Bad Request",
  "message": "Client error",
  "statusCode": 400
}
```

#### POST /api/organisations/:orgId/users

**Description:** Adds a user to a particular organisation.

**Request Body:**

```bash
{
  "userId": "string"
}
```

**Successful Response:**

```bash
{
  "status": "success",
  "message": "User added to organisation successfully"
}
```

## Validation Errors

**Example Validation Error Response:**

```bash
{
  "errors": [
    {
      "field": "string",
      "message": "string"
    }
  ]
}
```

## Unit Testing

**Test Scenarios:**

1. **Register User Successfully with Default Organisation:**
   - Ensure a user is registered successfully when no organisation details are provided.
   - Verify the default organisation name is correctly generated.
   - Check that the response contains the expected user details and access token.

2. **Log the User In Successfully:**
   - Ensure a user is logged in successfully when valid credentials are provided.
   - Check that the response contains the expected user details and access token.

3. **Fail If Required Fields Are Missing:**
   - Test cases for each required field (firstName, lastName, email, password) missing.
   - Verify the response contains a status code of 422 and appropriate error messages.

4. **Fail if There's Duplicate Email or UserID:**
   - Attempt to register two users with the same email.
   - Verify the response contains a status code of 422 and appropriate error messages.