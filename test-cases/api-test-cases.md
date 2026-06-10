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



