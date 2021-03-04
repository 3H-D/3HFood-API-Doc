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
                "id": 18,
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



## Get Warehouse storage

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getWarehouseStorage');
let data:{
  categoryId: -1,
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
            "id": 17,
            "name": "Produit test",
            "image_url": "https://google.fr",
            "ean": "123456431334545"
        }
    ]
}
```

This endpoint allows you to fetch products filtered by warehouse stock state (categoryId), with an offset for padding. Every parameter is mandatory.

### HTTP Request

`GET supermarket-getWarehouseStorage`

### Query Parameters

| Parameters | Type | Description                              |
| ---------- | ---- | ---------------------------------------- |
| categoryId | Int  | Id of the warehouse stock filter applied |
| Offset     | Int  | Offset applied for pagination            |



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



## Get Product Details

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getProductDetails');
let data:{
  productId: 17,
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
        "product": {
            "id": 17,
            "ean": "123456431334545",
            "linked_wholesaler": 1,
            "image_url": "https://google.fr",
            "name": "Produit test",
            "length": 53,
            "width": 25,
            "height": 8,
            "weight": 288,
            "selling_unit": "2x12l",
            "legal_unit_price": "1,38€/L",
            "professional_price": 55.8,
            "retail_price": 0,
            "vat_class": 5.5,
            "composition": "Juste de l'air",
            "linked_category": 2
        },
        "warehouse_stock": [
            {
                "id": 4,
                "product_id": 17,
                "supermarket_id": 4,
                "storage_alley": "E3",
                "quantity": 0,
                "perish_date": "2021-03-10"
            }
        ],
        "client_stock": [
            {
                "id": 1,
                "product_id": 17,
                "supermarket_id": 4,
                "quantity": 12,
                "perish_date": "2021-03-03"
            }
        ]
    }
}
```

This endpoint allows you to fetch product details, as well as the warehouse stock lines, and the client stock lines, order by perishment date from closer to more far away from perishment.

### HTTP Request

`GET supermarket-getProductDetails`

### Query Parameters

| Parameters | Type | Description                                     |
| ---------- | ---- | ----------------------------------------------- |
| productId  | Int  | Id of the product you want to fetch details for |



## Update product

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-updateProduct');
let data:{
  	productId: 17,
    warehouse: {
      editionLines: [
        {
          id: 4,
          quantity: 150,
          perishDate: "2021-03-20"
        }
      ],
        newLines: [
          {
            quantity: 100,
            perishDate: "2021-04-01"
          }
        ],
          deletionLines: []
    },
      client: {
        editionLines: [
          {
            id: 4,
            quantity: 150,
            perishDate: "2021-03-20"
          }
        ],
          newLines: [
            {
              quantity: 100,
              perishDate: "2021-04-01"
            }
          ],
            deletionLines: []
    }
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

This endpoint allows you to update product warehouse stock and client stock.

### HTTP Request

`GET supermarket-updateProduct`

### Query Parameters

| Parameters              |            | Type       | Description                                                  |
| ----------------------- | ---------- | ---------- | ------------------------------------------------------------ |
| productId               |            | Int        | Id of the product you want to fetch details for              |
| warehouse               |            | Object     | Object containing all line editions                          |
| warehouse.editionLines  |            | Array      | Array containing all edited warehouse stock lines in product |
|                         | id         | Int        | Id of the edited stock line                                  |
|                         | quantity   | Int        | New quantity value for the line                              |
|                         | perishDate | Date       | New perish date for the line                                 |
| warehouse.newLines      |            | Array      | Array containing all inserted warehouse stock lines for product |
|                         | quantity   | Int        | Inserted quantity                                            |
|                         | perishDate | Date       | Inserted perish date                                         |
| warehouse.deletionLines |            | Array<Int> | Array of deleted stock line ids                              |
|                         |            |            |                                                              |
| client                  |            | Object     | Object containing all line editions                          |
| client.editionLines     |            | Array      | Array containing all edited warehouse stock lines in productObject containing all line editions |
|                         | id         | Int        | Id of the edited stock line                                  |
|                         | quantity   | Int        | New quantity value for the line                              |
|                         | perishDate | Date       | New perish date for the line                                 |
| warehouse.newLines      |            | Array      | Array containing all inserted warehouse stock lines for product |
|                         | quantity   | Int        | Inserted quantity                                            |
|                         | perishDate | Date       | Inserted perish date                                         |
| warehouse.deletionLines |            | Array<Int> | Array of deleted stock line ids                              |



## Get Client Stock Detail

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getClientStockDetails');
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
                "id": 17,
                "name": "Produit test",
                "image_url": "https://google.fr",
                "ean": "123456431334545"
            }
        ],
        "categories": [
            {
                "category_product_count": 1,
                "category_name": "En rupture",
                "category_id": 0
            },
            {
                "category_product_count": 0,
                "category_name": "Bientôt en rupture",
                "category_id": -1
            },
            {
                "category_product_count": 0,
                "category_name": "En stock",
                "category_id": -2
            }
        ]
    }
}
```

This endpoint allows you to fetch client stock filters and the first 10 products from the first filter (out of stock products)

### HTTP Request

`GET supermarket-getClientStockDetails`

### Query Parameters

| Parameters | Type | Description                                     |
| ---------- | ---- | ----------------------------------------------- |
| productId  | Int  | Id of the product you want to fetch details for |



## Get Client Stock

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getClientStock');
let data = {
  categoryId: 0,
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
            "id": 17,
            "name": "Produit test",
            "image_url": "https://google.fr",
            "ean": "123456431334545"
        }
    ]
}
```

This endpoint allows you to fetch client stock filters and the first 10 products from the first filter (out of stock products)

### HTTP Request

`GET supermarket-getClientStock`

### Query Parameters

| Parameters | Type | Description                           |
| ---------- | ---- | ------------------------------------- |
| categoryId | Int  | Id of the client stock product filter |
| offset     | Int  | Value of the applied offset           |



## Get perishment dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getOrdersDashboard');
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
                "id": 18,
                "name": "Produit test",
                "ean": "123456431334545",
                "image_url": "https://google.fr",
                "perished_warehouse_quantity": 100,
                "closest_perishment_date": "2021-02-24",
                "perished_client_quantity": 0,
                "closest_client_date": null
            },
            {
                "id": 17,
                "name": "Produit test",
                "ean": "123456431334545",
                "image_url": "https://google.fr",
                "perished_warehouse_quantity": 0,
                "closest_perishment_date": null,
                "perished_client_quantity": 0,
                "closest_client_date": null
            }
        ],
        "categories": [
            {
                "category_product_count": 1,
                "category_name": "Périmés",
                "category_id": 0
            },
            {
                "category_product_count": 2,
                "category_name": "Périment dans -7j",
                "category_id": -1
            },
            {
                "category_product_count": 0,
                "category_name": "Périment dans -14j",
                "category_id": -2
            }
        ]
    }
}
```

This endpoint allows you to fetch the perishment dashboard, with the first 10 products of the first filter, and all the filters for the side selector.

### HTTP Request

`GET supermarket-getOrdersDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |



## Get perishment

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-getPerishments');
let data = {
  categoryId: 0,
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
            "id": 17,
            "name": "Produit test",
            "ean": "123456431334545",
            "image_url": "https://google.fr",
            "perished_warehouse_quantity": null,
            "closest_perishment_date": null,
            "perished_client_quantity": null,
            "closest_client_date": null
        },
        {
            "id": 18,
            "name": "Produit test",
            "ean": "123456431334545",
            "image_url": "https://google.fr",
            "perished_warehouse_quantity": 100,
            "closest_perishment_date": "2021-02-24",
            "perished_client_quantity": null,
            "closest_client_date": null
        }
    ]
}
```

This endpoint allows you to fetch filtered perishment products

### HTTP Request

`GET supermarket-getPerishments`

### Query Parameters

| Parameters | Type | Description                  |
| ---------- | ---- | ---------------------------- |
| categoryId | Int  | Id of the perishments filter |
| offset     | Int  | Value of the applied offset  |



## Delete perished stock

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('supermarket-deletePerishedProductStock');
let data = {
  productId: 17
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

This endpoint allows you to delete all perished stock lines from warehouse and client supermarket stock

### HTTP Request

`GET supermarket-deletePerishedProductStock`

### Query Parameters

| Parameters | Type | Description                                           |
| ---------- | ---- | ----------------------------------------------------- |
| productId  | Int  | Id of the product from whom remove the perished stock |

