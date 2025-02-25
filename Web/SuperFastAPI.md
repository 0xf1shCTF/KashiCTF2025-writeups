Going to /docs shows you the documentation for the api.

When we create a user

``` json
{
  "message": {
    "fname": "string",
    "lname": "string",
    "email": "string",
    "gender": "string",
    "role": "guest"
  }
}
```

this is their information. role is not changeable not matter what. However when we call /update we can pass in role:"admin" and set our user to admin. This allows us to retrieve the flag.

``` json
{
  "message": "KashiCTF{m455_4551gnm3n7_ftw_BJeO36y31}"
}
```
