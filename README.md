# Camero API documentation

## Define Api Host
```bash
CAMERO_API_HOST="http://localhost:8084/camero/api"
```

## For login in server : /login
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
**Status code**: 412

***Error type***: if email is null
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "Email is null."
        "suppressed": [0]
        "message": "Email is null."
    }
}
```
***Error type***: if password is null
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "Password is null."
        "suppressed": [0]
        "message": "Password is null."
    }
}
```
***Error type***: if email and password dont exist in database
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```

## For logout from server : /logout

```bash
curl -H "Content-Type: application/json" \
     -X DELETE \
     --cookie "userId=value;tokenValue=value"  "$CAMERO_API_HOST/logout"
```

### Response
#### Success
**Status code**: 202
```bash
{
  "content":  null,
  "error": null
}
```

### Error
**Status code**: 412

***Error type***: if there is no user ID value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "User ID is null."
        "suppressed": [0]
        "message": "User ID is null."
    }
}
```

***Error type***: if there is no token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "Value is null."
        "suppressed": [0]
        "message": "Value is null."
    }
}
```

## Get user profile : /user/{id}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/UserID"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
    "content": {
        "email": "value"
        "firstName": "value"
        "id": "value"
        "lastName": "value"
        "level": "value"
        "password": "value"
    }
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Delete user from server : /user/{id}
```bash
curl -H "Content-Type: application/json" \
     -X DELETE \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/UserID"
```
- **Note**: The user should be connected and has access privilege

### Response
#### Success
**Status code**: 202
```bash
{
  "content":  null,
  "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```
**Status code**: 403

***Error type***: if user dont have access privilege
```bash
 {
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Edit user in server : /user
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     --cookie "userId=value;tokenValue=value" \
     -d '{"email": "value","firstName": "value","id": "value","lastName":"value", "level": "value","password": "value"}' "$CAMERO_API_HOST/user"
```
- **Note**: The user should be connected and Password is the SHA-512 hash encoded in Base64


### Response
#### Success
**Status code**: 202
```bash
{
  "content":{
        "email": "UserEm@il",
        "firstName": "UserFirstName",
        "id": "UserID",
        "lastName": "UserName",
        "level": "UserLevel",
        "password": "UserPassword"
    },
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if user email dont exit in database or the email is already used
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "Email is not valid."
        "cause": null
        "localizedMessage": "Email is not valid."
        "suppressed": [0]
    }
}
```

**Status code**: 403

***Error type***: if current user not equal to target user 
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```

**Status code**: 412

***Error type***: if password is null
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "Password of user is null."
        "cause": null
        "localizedMessage": "Password of user is null."
        "suppressed": [0]
    }
}
```


## Retrive all users : /user/all
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/all"
```
- **Note**: The user should be connected 

### Response
#### Success
**Status code**: 200
```bash
{
	"content": [{
		"email": "value",
		"firstName": "value",
		"id": "value",
		"lastName": "value",
		"level": "value",
		"password": "value"
	}],
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

## Retrive user by email : /user/email/{email}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/email/EmailValue"
```
- **Note**: The user should be connected 

### Response
#### Success
**Status code**: 200
```bash
{
	"content": {
		"email": "EmailValue",
		"firstName": "value",
		"id": "value",
		"lastName": "value",
		"level": "value",
		"password": "value"
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

## Promote user : /user/promote/{userId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/promote/{userId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash

{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: User dont have privilege access
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Promote user : /user/demote/{userId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/demote/{userId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash

{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: User dont have privilege access
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Submit my candidat for a project : /project/candidacy/submit/{projectId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/candidacy/submit/{projectId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash

{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: User dont have privilege access
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Get candidat by ID : /project/candidacy/{candidacyId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/candidacy/{candidacyId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash

{
	"content": {
		"candidate": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"id": "value",
		"project": {
			... Project object data
		}
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

## Get project's candidacies : /project/candidacies/{projectId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/candidacies/{projectId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash
{
	"content": [{
		"candidate": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"id": "value",
		"project": {
		    ... Project object data
		}
	}],
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Reject candidacy : /project/candidacy/reject/{candidacyId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/candidacy/reject/{candidacyId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash
 
{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: if candidacy dont exist in database
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```

## Accept candidacy : /project/candidacy/accept/{candidacyId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/candidacy/accept/{candidacyId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash
 
{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: if candidacy dont exist in database
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Change owner of project : /project/changeOwnerRequest/submit/{projectId}/{ownerId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/changeOwnerRequest/submit/{projectId}/{ownerId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash
{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: if the project dont exit or the project is not supervised by the current user
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Accept request to be owner of project : /user/changeOwnerRequest/accept/{changeOwnerRequestId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/changeOwnerRequest/accept/{changeOwnerRequestId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: if change owner request dont exist in database
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Reject request to be owner of project : /user/changeOwnerRequest/reject/{changeOwnerRequestId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/changeOwnerRequest/reject/{changeOwnerRequestId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: if change owner request dont exist in database
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Get list of request for changing owner project : /user/changeOwnerRequests/{userId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/changeOwnerRequests/{userId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": [{
		"id": "value",
		"project": {
			"id": "value",
			"supervisor": {
				"email": "value",
				"firstName": "value",
				"id": "value",
				"lastName": "value",
				"level": "value",
				"password": "value"
			},
			"name": "value",
			"url": "value",
			"versions": [{
				"camelVersion": "value",
				"creationDate": value,
				"id": "value",
				"name": "value",
				"routes": [
					... 
				]
			}],
			"latestVersion": {
				"camelVersion": "value",
				"creationDate": value,
				"id": "value",
				"name": "value",
				"routes": []
			}
		},
		"user": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		}
	}],
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if userId is null
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "ID is null."
        "cause": null
        "localizedMessage": "ID is null."
        "suppressed": [0]
    }
}
```


## Delete project : project/{projectId}
```bash
curl -H "Content-Type: application/json" \
     -X DELETE \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/{projectId}"
```
- **Note**: The user should be connected and have privilege access

### Response
#### Success
**Status code**: 202
```bash
{
    "content": null
    "error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 403

***Error type***: if user dont have privilege access
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Retrive the list of all project : /project/all
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/all"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": [{
		"id": "value",
		"supervisor": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"name": "value",
		"url": "value",
		"versions": [{
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": [
			    ... Routes object data
			]
		}],
		"latestVersion": {
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": []
		}
	}],
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Retrive the list of user's project : /user/{userId}/projects
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/user/{userId}/projects"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": [{
		"id": "value",
		"supervisor": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"name": "value",
		"url": "value",
		"versions": [{
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": [
			    ... Routes object data
			]
		}],
		"latestVersion": {
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": []
		}
	}],
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Get project : /project/{projectId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/{projectId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": {
		"id": "value",
		"supervisor": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"name": "value",
		"url": "value",
		"versions": [{
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": [{
		        ... Routes object data
			}]
		}],
		"latestVersion": {
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": []
		}
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```


## Create Git project : /project/create/git
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     -d '{"url": "https://github.com/MohammedZebouchi/HelloCamel"}' \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/create/git"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 201
```bash
{
	"content": [{
		"id": "value",
		"supervisor": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"name": "value",
		"url": "value",
		"versions": [{
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": [{
		        ... Routes object data
			}]
		}],
		"latestVersion": {
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": []
		}
	}],
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if there is no route founded in repository .
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "No route found in repository."
        "cause": null
        "localizedMessage": "No route found in repository."
        "suppressed": [0]
    }
}
```

***Error type***: if there is git issue.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "null"
        "cause": null
        "localizedMessage": "null"
        "suppressed": [0]
    }
}
```


## Create Git project : /project/update/{projectId}
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/project/update/{projectId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 201
```bash
{
	"content": {
		"id": "value",
		"supervisor": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"name": "value",
		"url": "value",
		"versions": [{
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": [{
		        ... Routes object data
			}]
		}],
		"latestVersion": {
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": []
		}
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if project dont exist in database.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "Project not found!"
        "cause": null
        "localizedMessage": "No route found in repository."
        "suppressed": [0]
    }
}
```

***Error type***: if there is git issue.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "null"
        "cause": null
        "localizedMessage": "null"
        "suppressed": [0]
    }
}
```

**Status code**: 4O3

***Error type***: if the current user is not the owner of project.
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Get route by id : /route/{routeId}
```bash
curl -H "Content-Type: application/json" \
     -X Get \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/route/{routeId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": {
		"actions": ["value"],
		"fromEndpoint": "value",
		"id": "value",
		"name": "value",
		"services": ["value"],
		"tags": ["value"]
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if route id dont exist.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "ID is null."
        "cause": null
        "localizedMessage": "ID is null."
        "suppressed": [0]
    }
}
```


## Get project by route : /route/project/{routeId}
```bash
curl -H "Content-Type: application/json" \
     -X Get \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/route/project/{routeId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": {
		"id": "value",
		"supervisor": {
			"email": "value",
			"firstName": "value",
			"id": "value",
			"lastName": "value",
			"level": "value",
			"password": "value"
		},
		"name": "value",
		"url": "value",
		"versions": [{
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": [{
		        ... Routes object data
			}]
		}],
		"latestVersion": {
			"camelVersion": "value",
			"creationDate": value,
			"id": "value",
			"name": "value",
			"routes": []
		}
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if route id dont exist.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "ID is null."
        "cause": null
        "localizedMessage": "ID is null."
        "suppressed": [0]
    }
}
```


## Get route description in XML : /route/xml/{routeId}
```bash
curl -H "Content-Type: application/json" \
     -X Get \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/route/xml/{routeId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": "XML value",
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if route id dont exist.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "ID is null."
        "cause": null
        "localizedMessage": "ID is null."
        "suppressed": [0]
    }
}
```


## Update route : /route/update
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     -d '{"actions": ["value"],"fromEndpoint": "value","id": "value","name": "value","services": ["value"],"tags": ["value"]}' \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/route/update"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": {
		"actions": ["value"],
		"fromEndpoint": "value",
		"id": "value",
		"name": "value",
		"services": ["value"],
		"tags": ["value"]
	},
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if route id dont exist.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "ID is null."
        "cause": null
        "localizedMessage": "ID is null."
        "suppressed": [0]
    }
}
```

**Status code**: 4O3

***Error type***: if the current user is not the owner of project.
```bash
{
    "content": null
    "error": {
        "type": "forbidden"
        "cause": null
        "localizedMessage": null
        "suppressed": [0]
        "message": null
    }
}
```


## Get version description in XML : /version/xml/versionId}
```bash
curl -H "Content-Type: application/json" \
     -X GET \
     --cookie "userId=value;tokenValue=value" "$CAMERO_API_HOST/version/xml/versionId}"
```
- **Note**: The user should be connected

### Response
#### Success
**Status code**: 202
```bash
{
	"content": "XML value",
	"error": null
}
```

### Error
**Status code**: 401

***Error type***: if there is no user ID value or token value in Cookie
```bash
{
    "content": null
    "error": {
        "type": "user"
        "cause": null
        "localizedMessage": "null."
        "suppressed": [0]
        "message": "null."
    }
}
```

**Status code**: 412

***Error type***: if version dont exist in database.
```bash
{
    "content": null
    "error": {
        "type": "argument"
        "message": "Version ID is null."
        "cause": null
        "localizedMessage": "Version ID is null.."
        "suppressed": [0]
    }
}
```


## Make simple search : /search/simple
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     -d '{ "terms" : [value], smartSearch : boolean}' "$CAMERO_API_HOST/search/simple"
```

### Response
#### Success
**Status code**: 200
```bash
{
    "projects": [
        ... Project list data
    ]
    "routes": [
        ... Route list data
    ]
    "users": [
        ... User list data
    ]
}
```




## Make advanced search : /search/advanced
```bash
curl -H "Content-Type: application/json" \
     -X POST \
     -d '{ "routeSearch" : {"fromEndpoint": ["value"], "services": ["value"],"tags": ["value"]} 
, "userSearch" : {"firstName": ["value"], "lastName": ["value"]}
, "routeSearch" : {"projectName": ["value"], "camelVersion": ["value"],"tags": ["value"]} }' "$CAMERO_API_HOST/search/advanced"
```

### Response
#### Success
**Status code**: 200
```bash
{
    "projects": [
        ... Project list data
    ]
    "routes": [
        ... Route list data
    ]
    "users": [
        ... User list data
    ]
}
```
