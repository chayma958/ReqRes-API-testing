# API TEST CASES — ReqRes QA Collection

**Project:** API Testing using Postman
**API:** ReqRes
**Type:** Functional + Negative + Boundary Testing
**Tool:** Postman

---

# 1. FUNCTIONAL TEST CASES (P0)

---

## TC-F01 — Verify GET users list returns correct response

**API Endpoint:** `GET /api/users?page=2`
**Method:** GET

### Preconditions:

API is available

### Test Steps:

1. Send GET request to `/api/users?page=2`
2. Validate response body

### Expected Result:

* Status code = 200
* Response contains `data` array
* Each user object contains:

  * id
  * email
  * first_name
  * last_name

---

## TC-F02 — Verify GET single user by ID

**API Endpoint:** `GET /api/users/2`
**Method:** GET

### Test Steps:

1. Send request to `/api/users/2`

### Expected Result:

* Status code = 200
* Response contains user object
* User ID = 2
* Email field is not null

---

## TC-F03 — Verify CREATE user

**API Endpoint:** `POST /api/users`
**Method:** POST

### Test Data:

```json
{
  "name": "{{$randomFirstName}}",
  "job": "qa tester"
}
```

### Expected Result:

* Status code = 201 (or 200 depending on API behavior)
* Response contains:

  * name
  * job
  * id
  * createdAt timestamp

---

## TC-F04 — Verify UPDATE user (PATCH)

**API Endpoint:** `PATCH /api/users/2`

### Test Data:

```json
{
  "name": "random",
  "job": "senior qa engineer"
}
```

### Expected Result:

* Status code = 200 or 204
* Response contains updated fields (if returned)
* Job = "senior qa engineer"

---

## TC-F05 — Verify DELETE user

**API Endpoint:** `DELETE /api/users/2`

### Expected Result:

* Status code = 204
* Response body is empty

---

# 2. NEGATIVE TEST CASES (P0)

---

## TC-N01 — Login without password

**API Endpoint:** `POST /api/login`

### Test Data:

```json
{
  "email": "peter@klaven"
}
```

### Expected Result:

* Status code = 400
* Error message = "Missing password"

---

## TC-N02 — Invalid email format

**API Endpoint:** `POST /api/login`

### Test Data:

```json
{
  "email": "invalid-email",
  "password": "123"
}
```

### Expected Result:

* Status code = 400
* Error message is returned

---

## TC-N03 — Invalid user ID format

**API Endpoint:** `GET /api/users/abc`

### Expected Result:

* Status code = 404

---

# 3. BOUNDARY TEST CASES (P1)

---

## TC-B01 — Empty payload for user creation

**API Endpoint:** `POST /api/users`

### Test Data:

```json
{}
```

### Expected Result:

* Status code = 200 / 201 / 400 / 422 (system dependent)
* API does NOT crash

---

## TC-B02 — Wrong data types

**API Endpoint:** `POST /api/users`

### Test Data:

```json
{
  "name": 12345,
  "job": true
}
```

### Expected Result:

* Status code = 200 / 400 / 422
* System handles invalid types safely

---

## TC-B03 — Large input values

**API Endpoint:** `POST /api/users`

### Test Data:

```json
{
  "name": "aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa",
  "job": "bbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbbb"
}
```

### Expected Result:

* API does not crash
* Either success or validation error returned

---


Here are your **E2E API TEST CASES** written in the same style as your document (clean, QA-ready, and consistent):

---

# 4. END-TO-END (E2E) API CHAINING TEST CASES (P0)

---

## TC-E01 — Verify full user lifecycle (CREATE → GET → UPDATE → DELETE)

**API Flow:**

1. Create User → save `userId`
2. Get User using `userId`
3. Update User using `userId`
4. Delete User using `userId`

---

### Test Steps:

#### Step 1 — CREATE User

* Send `POST /api/users`
* Store returned `id` as `userId`

### Expected Result:

* Status code = 201
* Response contains:

  * id
  * name
  * job
  * createdAt

---

#### Step 2 — GET User

* Send `GET /api/users/{{userId}}`

### Expected Result:

* Status code = 200
* Response contains user data
* User ID matches stored `userId`

---

#### Step 3 — UPDATE User

* Send `PATCH /api/users/{{userId}}`

### Test Data:

```json
{
  "name": "updated name",
  "job": "senior qa engineer"
}
```

### Expected Result:

* Status code = 200 or 204
* Response contains updated fields (if API returns body)
* Job = "senior qa engineer"

---

#### Step 4 — DELETE User

* Send `DELETE /api/users/{{userId}}`

### Expected Result:

* Status code = 204
* Response body is empty
* User is considered deleted in workflow

---


