# Status codes reference

Observed on a local Toolshop (Docker), 2026-11-27. "Expectation Source" says where the expected code comes from.

| Endpoint | Test Case | Observed Code | Message / Body | Expectation Source |
|---|---|---:|---|---|
| `POST /users/login` | Valid credentials | 200 | `access_token`, `token_type`, `expires_in` | Swagger |
| `POST /users/login` | Incorrect password | 401 | `{"error": "Unauthorized"}` | HTTP convention |
| `POST /users/login` | Empty body | 401 | `{"error": "Invalid login request"}` | HTTP convention (400/422 expected) |
| `POST /users/login` | Too many failed attempts | 423 | `{"error": "Account locked, too many failed attempts..."}` | Not documented |
| `GET /users/me` | Valid token | 200 | user data | Swagger |
| `POST /users/register` | Valid new user | 201 | created user (id, email) | Swagger |
| `POST /users/register` | Leaked password | to confirm | `{"password": ["The given password has appeared in a data leak..."]}` | Not documented |
| `GET /products` | List | 200 | `data` array | Swagger |
| `GET /products/{id}` | Existing id | 200 | product | Swagger |
| `GET /products/{id}` | Non-existent id | 404 | `{"message": "Requested item not found"}` | HTTP convention |
| `POST /users/register` | Leaked password | 422 | `{"password": ["The given password has appeared in a data leak. Please choose a different password."]}| Not documented (Swagger lists 400 and 409) |