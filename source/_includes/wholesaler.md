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

| Parameters              | Child              | Type   | Description                                                  |
| ----------------------- | ------------------ | ------ | ------------------------------------------------------------ |
| product                 |                    | Object | A product object as described in the project                 |
|                         | ean                | String | EAN code of the product. No verification is done for it to be unique, so for the POC we'll assume it'll be ok |
|                         | imageUrl           | String | Link to the product image                                    |
|                         | name               | String | Name of the product as shown to final customers              |
|                         | length             | Float  | Length of the product (can be useful in the future)          |
|                         | width              | Float  | Width of the product (can be useful in the future)           |
|                         | height             | Float  | Height of the product (can be useful in the future)          |
|                         | weight             | Float  | Weight of the product (can be useful in the future)          |
|                         | sellingUnit        | String | The packaging size of the product. E.g: 12x1L, 200g, 1.5L, 1.5kg |
|                         | legalUnitPrice     | String | Price pet legal quantity. Eg: 2.00€/kg, 1.13€/L              |
|                         | professionnalPrice | Float  | Price as sold to supermarkets                                |
|                         | vatClass           | Float  | VAT value                                                    |
|                         | composition        | String | Ingredients of the product                                   |
|                         | categoryId         | Int    | Product Category ID                                          |
| stockMovements          |                    | Object | Map containing all stock movements done client side          |
| stockMovements.addition |                    | Array  | Array containing all added stock lines stored as objects     |
|                         | quantity           | Int    | Quantity on the stock line                                   |
|                         | perishDate         | Date   | Perish date on the stock line, format YYYY-MM-DD             |



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

### HTTP Request

`GET wholesaler-updateProduct`

### Query Parameters

| Parameters               | Child              | Type   | Description                                                  |
| ------------------------ | ------------------ | ------ | ------------------------------------------------------------ |
| product                  |                    | Object | A product object as described in the project                 |
|                          | ean                | String | EAN code of the product. No verification is done for it to be unique, so for the POC we'll assume it'll be ok |
|                          | imageUrl           | String | Link to the product image                                    |
|                          | name               | String | Name of the product as shown to final customers              |
|                          | length             | Float  | Length of the product (can be useful in the future)          |
|                          | width              | Float  | Width of the product (can be useful in the future)           |
|                          | height             | Float  | Height of the product (can be useful in the future)          |
|                          | weight             | Float  | Weight of the product (can be useful in the future)          |
|                          | sellingUnit        | String | The packaging size of the product. E.g: 12x1L, 200g, 1.5L, 1.5kg |
|                          | legalUnitPrice     | String | Price pet legal quantity. Eg: 2.00€/kg, 1.13€/L              |
|                          | professionnalPrice | Float  | Price as sold to supermarkets                                |
|                          | vatClass           | Float  | VAT value                                                    |
|                          | composition        | String | Ingredients of the product                                   |
|                          | categoryId         | Int    | Product Category ID                                          |
| stockMovements           |                    | Object | Map containing all stock movements done client side          |
| stockMovements.addition  |                    | Array  | Array containing all added stock lines stored as objects     |
|                          | quantity           | Int    | Quantity on the stock line                                   |
|                          | perishDate         | Date   | Perish date on the stock line, format YYYY-MM-DD             |
| stockMovements.changes   |                    | Array  | Array containing all edited stock lines stored as objects    |
|                          | id                 | Int    | Id of edited line                                            |
|                          | quantity           | Int    | New quantity of edited stock line                            |
|                          | perishDate         | Date   | New perish date on the stock line, format YYYY-MM-DD         |
| stockMovements.deletions |                    | Array  | Array of deleted ids                                         |
