# Client

## Create cart

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-cart-createCart');
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
        "cart_id": "9ae821ec-b447-4abf-bd28-126518acad81"
    }
}
```

This endpoint generates a cart id, store it to your user profile, and return it to you.

**Be careful, it will overwrite any existing cart uuid on your user profile.**

### HTTP Request

`GET client-cart-createCart`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |



## Add product to cart

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-cart-addProductToCart');
let data = {
  productId: 18,
  supermarketId: 4,
  quantity: 1
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
        "line_items": [
            {
                "quantity": 4,
                "applied_coupon": 80,
                "carbon_footprint": 0,
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Deuxième produit",
                "line_value": 119.5999984741211,
                "selling_unit": "2x12l",
                "brand_name": "MysteryTea",
              	"legal_unit_value": "18.64€/kg"
            }
        ],
        "cart_infos": {
            "number_of_items": 4,
            "cart_total": 39.599998474121094,
            "cart_footprint": 0
        }
    }
}
```

This endpoint adds a product to the cart, calculating the right coupon for the product if existing and eligible, updating the supermarket client stock adequately and returning all the infos for the cart to update it on client side.

**Be careful, it will overwrite any existing cart uuid on your user profile.**

### HTTP Request

`GET client-cart-addProductToCart`

### Query Parameters

| Parameters    | Type | Description                                          |
| ------------- | ---- | ---------------------------------------------------- |
| productId     | Int  | The id of the product to add to cart                 |
| supermarketId | Int  | The id of the supermarket the user is in buy mode in |
| quantity      | Int  | The quantity of products to add                      |



## Remove product from cart

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-cart-removeProductFromCart');
let data = {
  productId: 18,
  supermarketId: 4,
  quantity: -1
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
        "line_items": [
            {
                "quantity": 4,
                "applied_coupon": 80,
                "carbon_footprint": 0,
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Deuxième produit",
                "line_value": 119.5999984741211,
                "selling_unit": "2x12l",
                "brand_name": "MysteryTea",
              	"legal_unit_value": "18.64€/kg"
            }
        ],
        "cart_infos": {
            "number_of_items": 4,
            "cart_total": 39.599998474121094,
            "cart_footprint": 0
        }
    }
}
```

This endpoint removes a product from the cart, calculating the right coupon for the product if existing and eligible, updating the supermarket client stock adequately and returning all the infos for the cart to update it on client side.

**Be careful, it will overwrite any existing cart uuid on your user profile.**

### HTTP Request

`GET client-cart-removeProductFromCart`

### Query Parameters

| Parameters    | Type | Description                                          |
| ------------- | ---- | ---------------------------------------------------- |
| productId     | Int  | The id of the product to remove to cart              |
| supermarketId | Int  | The id of the supermarket the user is in buy mode in |
| quantity      | Int  | The quantity of products to remove                   |



## Launch cart payment

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-cart-startPayment');
let data = {
  supermarketId: 4
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

This endpoint launches the payment for the current user cart with a given supermarket id.

### HTTP Request

`GET client-cart-startPayment`

### Query Parameters

| Parameters    | Type | Description                                          |
| ------------- | ---- | ---------------------------------------------------- |
| supermarketId | Int  | The id of the supermarket the user is in buy mode in |



## Get orders

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-getOrders');
let data = {
  offset: 1
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
        "orders": [
            {
                "uuid": "91c8fcbb-cd36-4cb5-bb20-5c5811d623a4",
                "date": "2021-03-17 00:00:00",
                "products_count": 1,
                "vat_included_price": 9.9
            }
        ],
        "count": 1
    }
}
```

This endpoint fetches client orders ordered by date

### HTTP Request

`GET client-account-getOrders`

### Query Parameters

| Parameters | Type | Description                                  |
| ---------- | ---- | -------------------------------------------- |
| offset     | Int  | Offset applied to the results for pagination |

## Get order detail

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-getOrderDetail');
let data = {
  uuid: "91c8fcbb-cd36-4cb5-bb20-5c5811d623a4"
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
        "id": 4,
        "customer_id": 13,
        "date": "2021-03-17 00:00:00",
        "stringified_cart": [
            {
                "quantity": 1,
                "applied_coupon": 20,
                "carbon_footprint": 0,
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Deuxième produit",
                "line_value": 29.899999618530273,
                "selling_unit": "2x12l",
                "brand_name": "MysteryTea",
                "legal_unit_value": "18.54€/kg"
            }
        ],
        "products_count": 1,
        "carbon_footprint": 0,
        "vat_included_price": 9.9,
        "uuid": "91c8fcbb-cd36-4cb5-bb20-5c5811d623a4"
    }
}
```

This endpoint fetches order detail for a given uuid

### HTTP Request

`GET client-account-getOrderDetail`

### Query Parameters

| Parameters | Type   | Description                |
| ---------- | ------ | -------------------------- |
| uuid       | String | uuid of the order to fetch |



## Get perishments dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-getPerishmentDashboard');
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
        "perished_lines": [
            {
              	"id": 3,
                "perish_date": "2021-03-17",
                "quantity": 1,
                "product_id": 18,
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Deuxième produit",
                "first_name": "MysteryTea",
                "selling_unit": "2x12l"
            },
            {
              	"id": 4,
                "perish_date": "2021-03-17",
                "quantity": 1,
                "product_id": 18,
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Deuxième produit",
                "first_name": "MysteryTea",
                "selling_unit": "2x12l"
            }
        ],
        "perished_count": 2,
        "seven_days_count": 0
    }
}
```

This endpoint fetches the first 10 perished products in client stock, and fetch the count of perished products and 7 days perishments for pagination

### HTTP Request

`GET client-account-getPerishmentDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |

## Get perishments

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-getPerishments');
let data = {
  filterId: 0,
  offset: 0
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
    "result": [
        {
            "id": 5,
            "perish_date": "2021-03-17",
            "quantity": 1,
            "product_id": 18,
            "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
            "name": "Deuxième produit",
            "first_name": "MysteryTea",
            "selling_unit": "2x12l"
        },
        {
            "id": 4,
            "perish_date": "2021-03-17",
            "quantity": 1,
            "product_id": 18,
            "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
            "name": "Deuxième produit",
            "first_name": "MysteryTea",
            "selling_unit": "2x12l"
        }
    ]
}
```

This endpoint fetches 10 products of a given perishment category.

### HTTP Request

`GET client-account-getPerishments`

### Query Parameters

| Parameters | Type | Description                                                |
| ---------- | ---- | ---------------------------------------------------------- |
| filterId   | Int  | **0**: perished products, **-1**: Perishment within 7 days |
| offset     | Int  | Offset applied to the query                                |



## Get perishments

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-updatePerishmentLine');
let data = {
  stockLineId: 5,
  newQuantity: 2
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

This endpoint updates/deletes a client stock line

### HTTP Request

`GET client-account-updatePerishmentLine`

### Query Parameters

| Parameters  | Type | Description                                        |
| ----------- | ---- | -------------------------------------------------- |
| stockLineId | Int  | The stock line id to update                        |
| newQuantity | Int  | The new quantity value, or zero to delete the line |



## Get discounts

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-getDiscounts');
let data = {
  offset: 0,
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
        "discounts": [
            {
                "name": "Nouveau nom",
                "eligible_products": "\nDeuxième produit",
                "eligible_categories": "\nChampagnes, effervescents,\nBiscuits,\nChocolats, confiseries",
                "promotion_type": "PERCENTAGE",
                "value": 20,
                "bought_quantity": null,
                "paid_quantity": null,
                "start_date": "2021-03-01",
                "end_date": "2021-03-31"
            },
            {
                "name": "Deuxième promo",
                "eligible_products": null,
                "eligible_categories": "\nBiscuits,\nChips",
                "promotion_type": "BOGO",
                "value": null,
                "bought_quantity": 3,
                "paid_quantity": 2,
                "start_date": "2021-03-02",
                "end_date": "2021-03-12"
            }
        ],
        "count": 2
    }
}
```

This endpoint fetches 10 of the current discount sfor the user's prefered supermarket, and returns the total count of discounts for pagination.

### HTTP Request

`GET client-account-getDiscounts`

### Query Parameters

| Parameters | Type | Description                 |
| ---------- | ---- | --------------------------- |
| offset     | Int  | Offset applied to the query |



## Update email and/or phone

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-updateEmailAndPhone');
let data = {
  email: "florent.huran@outlook.fr",
  phone: "0785512671"
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

This endpoint updates the email and phone of a given user.

**Don't forget to update the firebase user email or all the future calls will fail**

### HTTP Request

`GET client-account-updateEmailAndPhone`

### Query Parameters

| Parameters | Type   | Description        |
| ---------- | ------ | ------------------ |
| email      | String | The new user email |
| phone      | String | The new user phone |



## Update address

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-updateAddress');
let data = {
  firstName: "Florent",
  lastName: "Huran",
  address: "144 rue NAtionale",
  city: "Lille",
  zipcode: "59000",
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

This endpoint updates the user address

### HTTP Request

`GET client-account-updateAddress`

### Query Parameters

| Parameters | Type   | Description             |
| ---------- | ------ | ----------------------- |
| firstName  | String | The new user first name |
| lastName   | String | The new user last name  |
| address    | String | The new user address    |
| city       | String | The new user city       |
| zipcode    | String | The new user zip code   |



## Update IBAN

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-updatePaymentMethod');
let data = {
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

This endpoint updates the user iban. 

### HTTP Request

`GET client-account-updatePaymentMethod`

### Query Parameters

| Parameters | Type   | Description                                    |
| ---------- | ------ | ---------------------------------------------- |
| iban       | String | The new iban to create the payment method from |
| bic        | String | For info purposes                              |



## Send Review

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-sendReview');
let data = {
  supermarketId: 4
  comment: "J'ai vraiment aimé ce magasin et toute la technologie qui va avec. Je reviendrai.",
  mark: 5
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

This endpoint updates the user iban. 

### HTTP Request

`GET client-account-sendReview`

### Query Parameters

| Parameters    | Type   | Description                                     |
| ------------- | ------ | ----------------------------------------------- |
| supermarketId | Int    | The id of the supermarket to link the review to |
| comment       | String | The comment written by the client               |
| mark          | Int    | The grade set by the user                       |



## Get client dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('client-account-getDashboard');
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
        "last_order_date": "2021-03-17 00:00:00",
        "last_order_price": 9.9,
        "total_carbon_footprint": 0,
        "total_perished_products": 1,
        "last_supermarket_name": "3HFood Seclin",
        "last_supermarket_id": 4,
        "ask_for_review_on_last_supermarket": 0,
        "first_name": "Florent"
    }
}
```

This endpoint fetches all the needed informations to display the client dashboard on application launch

### HTTP Request

`GET client-account-getDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |

