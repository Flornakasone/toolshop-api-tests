## POST /users/login: Swagger documents only the 200 response.
- Severity: medium
- What I tested: POST /users/login with a valid email and wrong password
- Expected (and source: Swagger / convention / unknown): No expectation written beforehand.
- Actual: 401 Unauthorized, body {"error": "Unauthorized"}.
- Decision: accepted. Reported as a documentation gap: error responses of the login endpoint are missing in the Swagger.
- How I verified: ran the test 'Login - invalid password' in Postman/Newman.

## POST /users/login: empty body returns 401 instead of 400/422.
- Severity: low
- What I tested: POST /users/login with an empty JSON body.
- Expected (and source: Swagger / convention / unknown): No expectation written beforehand. Reference: HTTP convention for missing fields (400 or 422).
- Actual: 401 Unauthorized, body {"error": "Invalid login request"}.
- Decision: accepted as observed behavior. Not confirmed as a bug: a validation error is reported with an authentication status code, which may be a design choice.
- How I verified: ran the test 'Login - empty body' in Postman/Newman

## POST /users/login: account locks after repeated failed logins (423)
- Severity: medium
- What I tested: repeated POST /users/login with a valid email and a wrong
  password, across several Newman runs with the same user.
- Expected (and source: Swagger / convention / unknown): No expectation written
  beforehand. The Swagger does not document the lockout or the 423 response.
- Actual: 423 Locked, body {"error": "Account locked, too many failed attempts. Please contact the administrator."}. The locked user could no longer log in, so the positive tests failed too.
- Decision: modified the test design. Negative login tests now use a throwaway user created in each run (Negative login tests use a throwaway user per run).
- How I verified: reproduced the lock by repeating the runs; after the change, consecutive Newman runs with 0 failures.

## Error responses use different body formats across endpoints
- Severity: medium
- What I tested: error responses of POST /users/login, POST /users/register (weak password) and GET /products/{id} (not found).
- Expected (and source: Swagger / convention / unknown): No expectation written beforehand. Reference: convention of a single error format across the API.
- Actual: three formats.
  - Login: {"error": "Unauthorized"}
  - Product not found: {"message": "Requested item not found"}
  - Register validation: {"password": ["The given password has appeared in a data leak..."]}
  The Swagger documents the register 401 as {"message": "Unauthorized"}, while the login returns the "error" key.
- Decision: accepted as observed behavior. Each test asserts the key of its own endpoint. Reported as an inconsistency: a client would need to handle three error formats.
- How I verified: ran the requests in Postman and Newman and compared the bodies.

## Negative login tests use a throwaway user per run
- Severity: low
- What I tested: negative login tests (wrong password) that must not lock the users the positive tests depend on.
- Expected (and source: Swagger / convention / unknown): negative tests can run repeatedly without locking accounts (own criterion: test repeatability).
- Actual: using the fixed users customer and customer2, the account got locked (423) and the tests could no longer run.
- Decision: create a new user in each run, with an email and a password generated in the pre-request script.
- Alternatives discarded: a single fixed user (it gets locked) and rotating customer3, 4, 5 (they run out and nobody can unlock them).
- Side effect: the password is also generated dynamically because the register endpoint rejects leaked passwords. This adds a dependency on an external service.
- How I verified: 5 consecutive Newman runs with 0 failures.