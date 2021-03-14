# COMMON - Back Office

## Check user exists & can access BO

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-userExistsForBackOffice');
let data = {
  email: "florent.huran@outlook.fr",
}
try {
  let result = await cloudFunction(data);
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "continueLogin": true,
        "shouldRenewPassword": false
    }
}
```

This endpoint checks if user exists and can access the BO.

### HTTP Request

`GET common-userExistsForBackOffice`

### Query Parameters

| Parameter | Required | Description                         |
| --------- | -------- | ----------------------------------- |
| Email     | false    | Email the user entered on the field |



## Get user details

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-getUser');
try {
  let result = await cloudFunction();
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "user": {
            "id": 1,
            "type": "WHOLESALER",
            "first_name": "Florent",
            "last_name": "Huran",
            "email": "florent.huran@outlook.fr",
            "must_renew_password": 0,
            "phone": null,
            "address": null,
            "zipcode": null,
            "city": null,
            "carbon_footprint": null,
            "iban": null,
            "bic": null,
            "stripe_customer_id": null,
            "stripe_mandate_id": "",
            "current_cart_uuid": null
        }
    }
}
```

This endpoint checks user details if authenticated in firebase.

### HTTP Request

`GET common-getUser`

### Query Parameters

| Parameter | Required | Description |
| --------- | -------- | ----------- |
| -         |          |             |



## Register client

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-createClient');
let data = {
		firstName: "Kamil",
    lastName: "Hammouche",
    phone: "0612345678",
    address: "25 rue Nationale",
    zipcode: "59000",
    city: "Lille",
  	iban: "DE89370400440532013000",
  	bic: "CMCIFR2A"
}
try {
  let result = await cloudFunction(data);
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "success": true
    }
}
```

First register user in firebase, then make the call with the firebase user so the API can retrieve his email, otherwise the call will crash.

This endpoint will create user in database, on stripe, create a payment method on stripe linked to the generated customer id, and create a SetupIntent for future payments of the client.

The BIC is only for information purpose, not taken into consideration by Stripe.

### HTTP Request

`GET common-createClient`

### Query Parameters

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| firstName | String | First name of user                       |
| lastName  | String | Last name of user                        |
| phone     | String | Phone of user                            |
| address   | String | Address of user                          |
| zipcode   | String | Zipcode of user - 5 chars max            |
| iban      | String | IBAN of the account to perform debits on |

## Register Supermarket

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-createSupermarket');
let data = {
    firstName: "Kamil",
    lastName: "Hammouche",
    phone: "0612345678",
    address: "25 rue Nationale",
    zipcode: "59000",
    city: "Lille",
}
try {
  let result = await cloudFunction(data);
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "success": true
    }
}
```

This endpoint takes all the parameters from registration to create a supermarket in db and on stripe side.

The email is fetched from the firebase auth token sent with the request.

### HTTP Request

`GET common-createSupermarket`

### Query Parameters

| Parameter | Type   | Description                   |
| --------- | ------ | ----------------------------- |
| firstName | String | First name of user            |
| lastName  | String | Last name of user             |
| phone     | String | Phone of user                 |
| address   | String | Address of user               |
| zipcode   | String | Zipcode of user - 5 chars max |
| city      | String | City of user                  |



## Register Cashier

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-createCashier');
let data = {
    supermarketEmail: "seclin@3hfood.fr",
}
try {
  let result = await cloudFunction(data);
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "success": true
    }
}
```

This endpoint takes all the parameters from registration and create the cashier user in db, and link it to the given supermarket.

### HTTP Request

`GET common-createCashier`

### Query Parameters

| Parameter        | Type   | Description                                     |
| ---------------- | ------ | ----------------------------------------------- |
| supermarketEmail | String | Email of the supermarket to link the cashier to |



## Register Privilegied User

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-createElevatedUser');
let data = {
    type: "SUPERMARKET",
    firstName: "Kamil",
    lastName: "Hammouche",
    phone: "0612345678",
    address: "25 rue Nationale",
    zipcode: "59000",
    city: "Lille",
}
try {
  let result = await cloudFunction(data);
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
        "success": true
    }
}
```

This endpoint takes all the parameters from registration and create the user object in database

### HTTP Request

`GET common-createClient`

### Query Parameters

| Parameter | Type   | Description                                                  |
| --------- | ------ | ------------------------------------------------------------ |
| firstName | String | First name of user                                           |
| lastName  | String | Last name of user                                            |
| phone     | String | Phone of user                                                |
| address   | String | Address of user                                              |
| zipcode   | String | Zipcode of user - 5 chars max                                |
| city      | String | City of user                                                 |
| type      | String | User type in: WHOLESALER - TAGMANAGER - STOCKMANAGER - SUPERADMIN |



## Set password reset

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('common-hasResetPassword');
try {
  let result = await cloudFunction();
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": {
       "success": true
    }
}
```

This endpoint set the flag 'needs password reset' to false. To be called when the user has reset his password and is well authenticated

### HTTP Request

`GET common-hasResetPassword`

### Query Parameters

| Parameter | Required | Description |
| --------- | -------- | ----------- |
| -         |          |             |

