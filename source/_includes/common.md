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
    iban: "FR12345678901234",
    bic: "CMCIFR2A",
    stripeCustomerId: "cst_324TYH5Y",
    stripeMandateId: "mdt_12345678T",
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

| Parameter        | Type   | Description                                        |
| ---------------- | ------ | -------------------------------------------------- |
| firstName        | String | First name of user                                 |
| lastName         | String | Last name of user                                  |
| phone            | String | Phone of user                                      |
| address          | String | Address of user                                    |
| zipcode          | String | Zipcode of user - 5 chars max                      |
| city             | String | City of user                                       |
| iban             | String | IBAN used for stripe registration                  |
| bic              | String | BIC used for stripe registration                   |
| stripeCustomerID | String | ID given by stripe at user registration            |
| stripeMandateId  | String | Mandate ID given when SEPA debit has been accepted |



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
| type      | String | User type in: WHOLESALER - SUPERMARKET - CASHIER - TAGMANAGER - STOCKMANAGER - SUPERADMIN |
