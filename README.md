# Camero API documentation

## Define Api Host
```bash
CAMERO_API_HOST="http://localhost:8084/camero/api"
```

## /login
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     -d '{"email": "UserEm@il", "password": "UserPassword"}' "$CAMERO_API_HOST/login"
```
- **Note**: Password is the SHA-512 hash encoded in Base64.

### Response
#### Success
**Status code**: 201
```bash
{
  "content":  {
    "expiryDate": <timestamp>,
    "id": "UserID",
    "user": {
      "email": "UserEm@il",
      "firstName": "UserFirstName",
      "id": "UserID",
      "lastName": "UserName",
      "level": "UserLevel",
      "password": "UserPassword"
    },
    "value": "value"
  },
  "error": null
}
```

### Error
__todo__


## /user/:id
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     "$CAMERO_API_HOST/user/UserID"
```
