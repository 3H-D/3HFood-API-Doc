# Client - Cart

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
  let result = await cloudFunction();
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
                "brand_name": "MysteryTea"
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
  let result = await cloudFunction();
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
                "brand_name": "MysteryTea"
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

