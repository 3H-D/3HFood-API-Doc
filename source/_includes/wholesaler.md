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
    professionnalPrice: 2.88,
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
    professionnalPrice: 2.88,
    vatClass: 5.5,
    composition: "Juste de l'air, on vend de l'air",
    categoryId: 2
  },
  stockMovements {
    addition: [
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
| stockMovements.addition  |                   | Array  | Array containing all added stock lines stored as objects     |
|                          | quantity          | Int    | Quantity on the stock line                                   |
|                          | perishDate        | Date   | Perish date on the stock line, format YYYY-MM-DD             |
| stockMovements.changes   |                   | Array  | Array containing all edited stock lines stored as objects    |
|                          | id                | Int    | Id of edited line                                            |
|                          | quantity          | Int    | New quantity of edited stock line                            |
|                          | perishDate        | Date   | New perish date on the stock line, format YYYY-MM-DD         |
| stockMovements.deletions |                   | Array  | Array of deleted ids                                         |



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

