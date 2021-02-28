# Supermarket

## Get all wholesalers

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getWholesalers');
try {
  let result = await cloudFunction();
} catch(e) {
  console.log(e);
}
```

> The above command returns JSON structured like this:

```json
{
    "result": [
        {
            "id": 1,
            "first_name": "MysteryTea",
            "last_name": "SASU"
        },
      	{
            "id": 2,
            "first_name": "Les bons gâteaux by MysteryTea",
            "last_name": "SASU"
        },
    ]
}
```

This endpoint allows you to fetch all existing wholesalers, useful for new order screen where the supermarket need to select a wholesaler to be able to fetch its products

### HTTP Request

`GET supermarket-getWholesalers`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |



## Create professional order

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-createOrder');
let data:{
  productLines: [
    {
      id: 18,
      quantity: 150
    }
  ],
  wholesalerId: 1
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
        "pdfUrl": "commande-pro/1/9390-055229-4545.pdf"
    }
}
```

This endpoints allow you to pass an array of order lines containing the product ID and a quantity, create the order in database, and fetch the PDF document url right after that to potentially open it in a new tab.

The call takes only product ids so no cheating is possible as the price set on the final order is the one in database.

### HTTP Request

`GET supermarket-getWholesalers`

### Query Parameters

| Parameters   |          | Type  | Description                                    |
| ------------ | -------- | ----- | ---------------------------------------------- |
| productLines |          | Array | Array of productLine objects                   |
|              | id       | Int   | ID of product linked to product line           |
|              | quantity | Int   | Quantity of product linked to the product line |
| wholesalerId |          | Int   | Id of the selected wholesaler                  |



## Get professional orders

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getOrders');
let data:{
  offset: 0
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
        "orders": [
            {
                "id": 9,
                "order_id": "9390-057749-4545",
                "wholesaler_id": 1,
                "supermarket_id": 4,
                "date": "2021-02-27 00:00:00",
                "products_count": 150,
                "vat_exempt_price": 8370,
                "pdf_url": "commande-pro/1/9390-057749-4545.pdf",
                "state": "PREPARE",
                "stringified_perishments": "[{\"productId\":18,\"quantity\":150,\"perishDate\":\"2021-02-28\"}]"
            }
        ],
        "totalOrders": 1
    }
}
```

This endpoint allows you to fetch professional orders linked to a supermarket with an offset. Every call returns you the total number of orders too for your front end logic on wether or not you can display next/previous buttons.

### HTTP Request

`GET supermarket-getOrders`

### Query Parameters

| Parameters | Type | Description               |
| ---------- | ---- | ------------------------- |
| Offset     | Int  | Offset applied to results |



## Set order as delivered

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-setOrderReceived');
let data:{
  orderId: 9
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

This endpoint allows you to set a professionnal order as delivered, and set correct stock lines into warehouse stock database with perishment dates.

### HTTP Request

`GET supermarket-setOrderReceived`

### Query Parameters

| Parameters | Type | Description                         |
| ---------- | ---- | ----------------------------------- |
| orderId    | Int  | Id of the order to set as delivered |



## Get warehouse stock dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getWarehouseStockDashboard');
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
        "products": [
            {
                "product_id": 18,
                "name": "Produit test",
                "image_url": "https://google.fr",
                "ean": "123456431334545"
            }
        ],
        "categories": [
            {
                "category_product_count": 1,
                "category_name": "Produits non assignés",
                "category_id": 0
            },
            {
                "category_product_count": 0,
                "category_name": "En rupture",
                "category_id": -1
            },
            {
                "category_product_count": 1,
                "category_name": "Bientôt en rupture",
                "category_id": -2
            },
            {
                "category_product_count": 0,
                "category_name": "En stock",
                "category_id": -3
            }
        ]
    }
}
```

This endpoint allows you to get the filters categories with labels and products count, and to get the first 10 products from the first filter category.

Un produit ne peut être que dans une seule catégorie. Ainsi, un produit en stock mais non assigné n'apparaîtra que dans la catégorie "non assigné" et pas dans la catégorie "en stock"

A product can only be part of one category. So if a product is in stock but is not yet assigned to a warehouse storage alley, then it will only appear in "Produits non assignés" category and not in "en stock" one.

### HTTP Request

`GET supermarket-getWarehouseStockDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |



## Set storage alley

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-setStorageAlley');
let data:{
  productId: 18,
  storageAlley: "G2"
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

This endpoint allows you to set a storage alley for an product. It'll update all the stock lines of the product with the new storage alley value. If no alley was existing, then the product will change category on refresh. If alley was existing, it'll be overriden.

Next professionnal orders of the same product will automatically be assigned to this storage alley.

### HTTP Request

`GET supermarket-setStorageAlley`

### Query Parameters

| Parameters   | Type   | Description                                   |
| ------------ | ------ | --------------------------------------------- |
| productId    | Int    | Id of the product to set the storage alley on |
| storageAlley | String | Value of the storage alley (max 31 chars)     |

