# Wholesaler

## Create product

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-createProduct');
let data = {
  product: {
    ean: "1234567890123",
    imageUrl: "https://google.fr",
    name: "Produit test",
    length: 12.9,
    width: 69.0,
    height: 88.0,
    weight: 125.7,
    sellingUnit: "12x1L",
    legalUnitPrice: "1,18€/L",
    professionalPrice: 2.88,
    vatClass: 5.5,
    composition: "Juste de l'air, on vend de l'air",
    categoryId: 2
  },
  stockMovements {
    addition: [{
				quantity: 120,
  			perishDate: "2022-02-20"
      }, 
    	{
				quantity: 200,
  			perishDate: "2022-02-18"
      }]
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

This endpoint creates a new product in database without checking if another EAN is already in base

### HTTP Request

`GET wholesaler-createProduct`

### Query Parameters

| Parameters              | Child             | Type   | Description                                                  |
| ----------------------- | ----------------- | ------ | ------------------------------------------------------------ |
| product                 |                   | Object | A product object as described in the project                 |
|                         | ean               | String | EAN code of the product. No verification is done for it to be unique, so for the POC we'll assume it'll be ok |
|                         | imageUrl          | String | Link to the product image                                    |
|                         | name              | String | Name of the product as shown to final customers              |
|                         | length            | Float  | Length of the product (can be useful in the future)          |
|                         | width             | Float  | Width of the product (can be useful in the future)           |
|                         | height            | Float  | Height of the product (can be useful in the future)          |
|                         | weight            | Float  | Weight of the product (can be useful in the future)          |
|                         | sellingUnit       | String | The packaging size of the product. E.g: 12x1L, 200g, 1.5L, 1.5kg |
|                         | legalUnitPrice    | String | Price pet legal quantity. Eg: 2.00€/kg, 1.13€/L              |
|                         | professionalPrice | Float  | Price as sold to supermarkets                                |
|                         | vatClass          | Float  | VAT value                                                    |
|                         | composition       | String | Ingredients of the product                                   |
|                         | categoryId        | Int    | Product Category ID                                          |
| stockMovements          |                   | Object | Map containing all stock movements done client side **- required even if empty** |
| stockMovements.addition |                   | Array  | Array containing all added stock lines stored as objects     |
|                         | quantity          | Int    | Quantity on the stock line                                   |
|                         | perishDate        | Date   | Perish date on the stock line, format YYYY-MM-DD             |



## Update product

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-updateProduct');
let data = {
  product: {
    id: 2,
    ean: "1234567890123",
    imageUrl: "https://google.fr",
    name: "Produit test",
    length: 12.9,
    width: 69.0,
    height: 88.0,
    weight: 125.7,
    sellingUnit: "12x1L",
    legalUnitPrice: "1,18€/L",
    professionalPrice: 2.88,
    vatClass: 5.5,
    composition: "Juste de l'air, on vend de l'air",
    categoryId: 2
  },
  stockMovements {
    additions: [
  		{
				quantity: 120,
  			perishDate: "2022-02-20"
      }, 
    	{
				quantity: 200,
  			perishDate: "2022-02-18"
      }
		],
    changes: [
      {
        id: 15,
        quantity: 11,
        perishDate: "2022-02-18"
   		}, 
     	{
        id: 22,
        quantity: 155,
        perishDate: "2022-02-18"
    	}
    ],
    deletions: [17, 24],
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

This endpoint creates a new product in database without checking if another EAN is already in base

### Important

Si une erreur est renvoyée de cet appel, et que cette erreur n'est pas une erreur INVALID_ARGUMENT, alors il y a eu un problème côté DB mais une partie des modifications ont été sauvegardées. On 

If this call raises an error not of type INVALID_ARGUMENT, it means there was a problem on the database side (most probably a stok modification on a stock line that no longer exists), but some product info had been saved anyway, so please **force refresh the page**

### HTTP Request

`GET wholesaler-updateProduct`

### Query Parameters

| Parameters               | Child             | Type   | Description                                                  |
| ------------------------ | ----------------- | ------ | ------------------------------------------------------------ |
| product                  |                   | Object | A product object as described in the project                 |
|                          | ean               | String | EAN code of the product. No verification is done for it to be unique, so for the POC we'll assume it'll be ok |
|                          | imageUrl          | String | Link to the product image                                    |
|                          | name              | String | Name of the product as shown to final customers              |
|                          | length            | Float  | Length of the product (can be useful in the future)          |
|                          | width             | Float  | Width of the product (can be useful in the future)           |
|                          | height            | Float  | Height of the product (can be useful in the future)          |
|                          | weight            | Float  | Weight of the product (can be useful in the future)          |
|                          | sellingUnit       | String | The packaging size of the product. E.g: 12x1L, 200g, 1.5L, 1.5kg |
|                          | legalUnitPrice    | String | Price pet legal quantity. Eg: 2.00€/kg, 1.13€/L              |
|                          | professionalPrice | Float  | Price as sold to supermarkets                                |
|                          | vatClass          | Float  | VAT value                                                    |
|                          | composition       | String | Ingredients of the product                                   |
|                          | categoryId        | Int    | Product Category ID                                          |
| stockMovements           |                   | Object | Map containing all stock movements done client side **- required even if empty** |
| stockMovements.additions |                   | Array  | Array containing all added stock lines stored as objects     |
|                          | quantity          | Int    | Quantity on the stock line                                   |
|                          | perishDate        | Date   | Perish date on the stock line, format YYYY-MM-DD             |
| stockMovements.changes   |                   | Array  | Array containing all edited stock lines stored as objects    |
|                          | id                | Int    | Id of edited line                                            |
|                          | quantity          | Int    | New quantity of edited stock line                            |
|                          | perishDate        | Date   | New perish date on the stock line, format YYYY-MM-DD         |
| stockMovements.deletions |                   | Array  | Array of deleted ids                                         |

## Update product

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-deleteProduct');
let data = {
  productId: 18,
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

This endpoint deletes the product from the wholesaler catalog and its associated stock lines. This does not delete the product from the supermarket side, as it can still have some in stock.

### HTTP Request

`GET wholesaler-deleteProduct`

### Query Parameters

| Parameters | Type | Description                 |
| ---------- | ---- | --------------------------- |
| productId  | Int  | Id of the product to delete |



## Get product detail

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getProductDetail');
let data = {
  productId: 18
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
            "id": 18,
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
            "composition": "Juste de l'eau",
            "linked_category": 4,
          	"category_name": "Biscuits"
        },
        "stockLines": [
            {
                "id": 49,
                "product_id": 18,
                "wholesaler_id": 1,
                "quantity": 9550,
                "perish_date": "2021-02-28"
            }
        ]
    }
}
```

This endpoint fetches all the product informations as well as all the stock lines ordered from more close to more far away from perishment date.

### HTTP Request

`GET wholesaler-getProductDetail`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |
|            |      |             |



## Get products dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getProductsDashboard');
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
                "image_url": "https://google.fr",
                "name": "Produit test",
                "ean": "123456431334545",
                "id": 18,
                "stock_quantity": 640
            },
            {
                "image_url": "https://google.fr",
                "name": "Produit test",
                "ean": "123456431334545",
                "id": 17,
                "stock_quantity": 0
            }
        ],
        "categories": [
            {
                "category_product_count": 2,
                "category_name": "Tous les produits",
                "category_id": 0
            },
            {
                "category_product_count": 1,
                "category_name": "En rupture",
                "category_id": -1
            },
            {
                "category_product_count": 0,
                "category_name": "Bientôt en rupture",
                "category_id": -2
            },
            {
                "category_product_count": 1,
                "category_name": "Biscuits",
                "category_id": 2
            },
            {
                "category_product_count": 4,
                "category_name": "Cafés",
                "category_id": 4
            }
        ]
    }
}
```

This endpoint has multiple interesting data inside of it.

It can fetch the first 10 products from the "all products" tab (which is the default one, so thumbs up for that)

It fetches product counts for total count, out of stock, and near out of stock for the side bar.

And it sends back an array of categories with product count and labels for side bar filtering

### HTTP Request

`GET wholesaler-getProductsDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |
|            |      |             |



## Get products

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getProducts');
let data = {
  categoryId: -2,
  offset: 10
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
            "image_url": "https://google.fr",
            "name": "Produit test",
            "ean": "123456431334545",
            "id": 18,
            "stock_quantity": 640
        },
        {
            "image_url": "https://google.fr",
            "name": "Produit test",
            "ean": "123456431334545",
            "id": 17,
            "stock_quantity": 0
        }
    ]
}
```

This endpoints allow you to fetch data with an offset and a category or a stock status filter (according to the filters list the api sent you on the previous call);

You'll get products by bunch of 10.

### HTTP Request

`GET wholesaler-getProducts`

### Query Parameters

| Parameters | Type | Description                                                  |
| ---------- | ---- | ------------------------------------------------------------ |
| categoryId | Int  | The filter category ID (sent in the previous call where you fetched all available filters) |
| offset     | Int  | The offset applied to the request                            |



## Get peremption dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getPerishmentDashboard');
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
                "image_url": "https://google.fr",
                "name": "Produit test",
                "ean": "123456431334545",
                "id": 18,
                "perished_quantity": 640
            }
        ],
        "categories": [
            {
                "category_name": "Périmés",
                "category_product_count": 1,
                "category_id": 0
            },
            {
                "category_name": "Périment dans -7 jours",
                "category_product_count": 0,
                "category_id": -1
            },
            {
                "category_name": "Périment dans -14 jours",
                "category_product_count": 0,
                "category_id": -2
            }
        ]
    }
}
```

This endpoint allow you to fetch the first 10 perished products and fetch the side filters with product count and filter id in order to perform other queries.

### HTTP Request

`GET wholesaler-getPerishmentDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |



## Get perishments

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getPerishments');
let data = {
  categoryId: -2,
  offset: 10
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
        "products": [
            {
                "image_url": "https://google.fr",
                "name": "Produit test",
                "ean": "123456431334545",
                "id": 18,
                "perished_quantity": 640
            }
        ],
        "categories": [
            {
                "category_name": "Périmés",
                "category_product_count": 1,
                "category_id": 0
            },
            {
                "category_name": "Périment dans -7 jours",
                "category_product_count": 0,
                "category_id": -1
            },
            {
                "category_name": "Périment dans -14 jours",
                "category_product_count": 0,
                "category_id": -2
            }
        ]
    }
}
```

This endpoint allows you to fetch 10 products from a set perishment date, with a given offset. You get the categoryId from the first call from peremptionDashboard

### HTTP Request

`GET wholesaler-getPerishments`

### Query Parameters

| Parameters | Type | Description                                                  |
| ---------- | ---- | ------------------------------------------------------------ |
| categoryId | Int  | The filter category ID (sent in the previous call where you fetched all available filters) |
| offset     | Int  | The offset applied to the request                            |



## Delete perishments for product

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-deletePerishedStock');
let data = {
  productId: 18,
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

This endpoint allows you to fetch 10 products from a set perishment date, with a given offset. You get the categoryId from the first call from peremptionDashboard

### HTTP Request

`GET wholesaler-deletePerishedStock`

### Query Parameters

| Parameters | Type | Description                                              |
| ---------- | ---- | -------------------------------------------------------- |
| productId  | Int  | The product ID from whom delete all perished stock lines |



## Get dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getDashboard');
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
        "to_prepare": 0,
        "in_delivery": 0,
        "out_of_stock_count": 0,
        "perishment_warnings": [
            {
                "first_name": "3HFood",
                "last_name": "Seclin",
                "products_count": 2,
                "quantity_count": 250,
                "total_price": 10649.999809265137
            }
        ]
    }
}
```

This endpoint allows you to fetch all the data for the wholesaler dashboard

### HTTP Request

`GET wholesaler-getDashboard`

### Query Parameters

| Parameters | Type | Description |
| ---------- | ---- | ----------- |



## Get tracker

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getTracking');
let data = {
  orderingId: 0,
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
    "result": {
        "tracker_lines": [
            {
                "id": 4,
                "first_name": "3HFood",
                "last_name": "Seclin",
                "near_perishment_references_count": 1,
                "perished_products_count": 250,
                "references_count": 2,
                "products_count": 350
            }
        ],
        "total_lines": 1
    }
}
```

This endpoint allows you to fetch tracker data to allow the wholesaler to see stock status on every supermarket he delivers. 

Without a defined supermarketId, it'll fetch the 10 first lines (or the 10 lines offseted by the offset value) of the tracker.

With a defined supermarketId, it'll only fetch this line of the tracker and no other.

### HTTP Request

`GET wholesaler-getTracking`

### Query Parameters

| Parameters    | Type | Description                                                  |
| ------------- | ---- | ------------------------------------------------------------ |
| orderingId    | Int  | Different values: **0**-Alphabetical order ascending, **1**-Near perishment references count descending, **2**-Perished products count descending |
| offset        | Int  | Offset value applied to the query                            |
| supermarketId | Int? | Optionnal supermarketId filter to only fetch this supermarket in particular. Useful for the search functionnality |



## Get tracker detail

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getTrackingDetail');
let data = {
  supermarketId: 4
  orderingId: 0,
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
    "result": {
        "lines": [
            {
                "product_id": 18,
                "name": "Deuxième produit",
                "ean": "123456431334545",
                "total_stock": 200,
                "nearest_perish_date": "2021-03-17",
                "near_perish_stock": 600
            },
            {
                "product_id": 17,
                "name": "Produit test",
                "ean": "123456431334545",
                "total_stock": 250,
                "nearest_perish_date": "2021-03-31",
                "near_perish_stock": 0
            }
        ],
        "total_lines": 2
    }
}
```

This endpoint allows you to fetch tracker detail for a specified supermarket Id, with ordering and offset.

### HTTP Request

`GET wholesaler-getTrackingDetail`

### Query Parameters

| Parameters    | Type | Description                                                  |
| ------------- | ---- | ------------------------------------------------------------ |
| supermarketId | Int  | SupermarketId to fetch the tracker detail lines from.        |
| orderingId    | Int  | Different values: **0**-Alphabetical order ascending, **1**-Total Stock ascending, **2**-Perished date from closest to far away, **3**-Stock quantity close to perish, descending |
| offset        | Int  | Offset value applied to the query                            |



## Get professionnal orders dashboard

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getOrdersDashboard');
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
                "order_id": "9390-057749-4545",
                "date": "2021-02-27 00:00:00",
                "products_count": 150,
                "vat_exempt_price": 8370,
                "pdf_url": "commande-pro/1/9390-057749-4545.pdf",
                "supermarket_name": "3HFood Seclin"
            }
        ],
        "categories": [
            {
                "category_product_count": 1,
                "category_name": "À préparer",
                "category_id": 0
            },
            {
                "category_product_count": 0,
                "category_name": "À expédier",
                "category_id": -1
            },
            {
                "category_product_count": 0,
                "category_name": "Expédiées",
                "category_id": -2
            },
            {
                "category_product_count": 0,
                "category_name": "Livrées",
                "category_id": -3
            },
            {
                "category_product_count": 1,
                "category_name": "Total",
                "category_id": -4
            }
        ]
    }
}
```

This endpoint allows you to fetch the first 10 orders to be in "PREPARE" state and the filters for the orders dashboard

### HTTP Request

`GET wholesaler-getOrdersDashboard

## Get orders by filter

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('wholesaler-getOrders');
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
            "order_id": "9390-057749-4545",
            "date": "2021-02-27 00:00:00",
            "products_count": 150,
            "vat_exempt_price": 8370,
            "pdf_url": "commande-pro/1/9390-057749-4545.pdf",
            "supermarket_name": "3HFood Seclin"
        }
    ]
}
```

This endpoint allows you to fetch wholesaler orders ordered by date and with a mandatory offset.

You have to pass a categoryId which is the filter id fetched from the ordersDashboard query.

### HTTP Request

`GET wholesaler-getOrders`

### Query Parameters

| Parameters | Type | Description                            |
| ---------- | ---- | -------------------------------------- |
| categoryId | Int  | Filter ID as fetched by dashboard call |
| offset     | Int  | Offset value applied to the query      |

