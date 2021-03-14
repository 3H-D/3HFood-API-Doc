# Search

## Search in wholesaler products

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-wholesalerProduct');
let data:{
  search: "test",
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
    "result": {
        "results": [
            {
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Produit test",
                "ean": "123456431334545",
                "id": 17,
                "stock_quantity": 9550
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to fetch products for a search query from the wholesaler products dashboard. It is filtered by the current category filter on the dashboard.

### HTTP Request

`GET search-wholesalerProduct`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| categoryId | Int    | Currently active category id                                 |
| offset     | Int    | Offset applied to results                                    |



## Search in categories (COMMON)

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-commonCategory');
let data:{
  search: "Bisc",
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
            "id": 2,
            "name": "Biscuits"
        }
    ]
}
```

This endpoints allows you to fetch categories responding to a query. Results are limited to 5.

### HTTP Request

`GET search-commonCategory`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |



## Search tracker line

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-wholesalerTrackingLines');
let data:{
  search: "secl",
  orderingId: 0,
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
        "results": [
            {
                "id": 4,
                "first_name": "3HFood",
                "last_name": "Seclin",
                "near_perishment_references_count": 1,
                "perished_products_count": 250,
                "references_count": 2,
                "products_count": 750
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to fetch tracker lines for a given query string.

### HTTP Request

`GET search-wholesalerTrackingLines`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| orderingId | Int    | Different values: **0**-Alphabetical order ascending, **1**-Near perishment references count descending, **2**-Perished products count descending |
| offset     | Int    | Applied offset to the query for pagination                   |



## Search tracker details

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-wholesalerTrackingDetail');
let data:{
  search: "deux",
  supermarketId: 4,
  orderingId: 0,
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
        "results": [
            {
                "product_id": 18,
                "name": "Deuxième produit",
                "ean": "123456431334545",
                "total_stock": 200,
                "nearest_perish_date": "2021-03-17",
                "near_perish_stock": 600
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to fetch tracker products for a given query string

### HTTP Request

`GET search-wholesalerTrackingLines`

### Query Parameters

| Parameters    | Type   | Description                                                  |
| ------------- | ------ | ------------------------------------------------------------ |
| search        | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| supermarketId | Int    | ID of the supermarket to fetch lines from                    |
| orderingId    | Int    | Different values: **0**-Alphabetical order ascending, **1**-Total Stock ascending, **2**-Perished date from closest to far away, **3**-Stock quantity close to perish, descending |
| offset        | Int    | Applied offset to the query for pagination                   |



## Search wholesaler orders

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-wholesalerOrders');
let data:{
  search: "secl",
  offset: 0,
  filterId: -3
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
        "results": [
            {
                "order_id": "9390-057749-4545",
                "date": "2021-02-27 00:00:00",
                "products_count": 150,
                "vat_exempt_price": 8370,
                "pdf_url": "commande-pro/1/9390-057749-4545.pdf",
                "supermarket_name": "3HFood Seclin"
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to fetch professionnal orders for a given query string

### HTTP Request

`GET search-wholesalerOrders`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| offset     | Int    | Applied offset to the query for pagination                   |
| filterId   | Int    | The current orders filter applied                            |



## Search wholesaler perishments

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-wholesalerPerishments');
let data:{
  search: "secl",
  offset: 0,
  filterId: -3
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
        "results": [
            {
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Deuxième produit",
                "ean": "123456431334545",
                "id": 18,
                "perished_quantity": 9550
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to fetch perishments for a given query string

### HTTP Request

`GET search-wholesalerPerishments`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| offset     | Int    | Applied offset to the query for pagination                   |
| filterId   | Int    | The current orders filter applied                            |



## Search supermarket orders

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-supermarketOrders');
let data:{
  search: "Myst",
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
        "results": [
            {
                "id": 9,
                "order_id": "9390-057749-4545",
                "wholesaler_id": 1,
                "supermarket_id": 4,
                "date": "2021-02-27 00:00:00",
                "products_count": 150,
                "vat_exempt_price": 8370,
                "pdf_url": "commande-pro/1/9390-057749-4545.pdf",
                "state": "DELIVERED",
                "stringified_perishments": "[{\"productId\":18,\"quantity\":150,\"perishDate\":\"2021-02-28\"}]",
                "professional_name": "MysteryTea SASU"
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to fetch supermarket orders with a given wholesaler name or order id

### HTTP Request

`GET search-supermarketOrders`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| offset     | Int    | Applied offset to the query for pagination                   |



## Search product for supermarket new order

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-supermarketProductOnOrder');
let data:{
  search: "Prod",
  wholesalerId: 1,
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
            "ean": "123456431334545",
            "name": "Produit test",
            "professional_price": 22.8
        },
        {
            "id": 18,
            "ean": "123456431334545",
            "name": "Deuxième produit",
            "professional_price": 55.8
        }
    ]
}
```

This endpoints allows you to get autocompletion for product search feature on new order screen, with price information for each suggestion in order to build the order properly.

### HTTP Request

`GET search-supermarketProductOnOrder`

### Query Parameters

| Parameters   | Type   | Description                                                  |
| ------------ | ------ | ------------------------------------------------------------ |
| search       | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| wholesalerId | Int    | The selected wholesaler id from the dropdown on new order screen |



## Search product for supermarket other screens

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-supermarketProductIdAndName');
let data:{
  search: "Prod"
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
            "name": "Produit test"
        },
        {
            "id": 18,
            "name": "Deuxième produit"
        }
    ]
}
```

This endpoints allows you to fetch product results on query for optimisation and pricing screens

### HTTP Request

`GET search-supermarketProductIdAndName`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |



## Search supermarket warehouse stock

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-supermarketWarehouseStock');
let data:{
  search: "Prod",
  categoryId: -3,
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
        "results": [
            {
                "id": 17,
                "name": "Produit test",
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "ean": "123456431334545"
            },
            {
                "id": 18,
                "name": "Deuxième produit",
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "ean": "123456431334545"
            }
        ],
        "count": 2
    }
}
```

This endpoints allows you to search for specific products on warehouse stock screen

### HTTP Request

`GET search-supermarketWarehouseStock`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| categoryId | Int    | The selected warehouse stock filter                          |
| offset     | Int    | Offset applied to the query for pagination                   |



## Search supermarket client stock

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-supermarketClientStock');
let data:{
  search: "Prod",
  categoryId: -2,
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
        "results": [
            {
                "id": 18,
                "name": "Deuxième produit",
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "ean": "123456431334545"
            },
            {
                "id": 17,
                "name": "Produit test",
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "ean": "123456431334545"
            }
        ],
        "count": 2
    }
}
```

This endpoints allows you to search for specific products in client stock

### HTTP Request

`GET search-supermarketClientStock`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| categoryId | Int    | The selected client stock filter                             |
| offset     | Int    | Offset applied to the query for pagination                   |



## Search supermarket perishments

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('search-supermarketPerishments');
let data:{
  search: "Prod",
  categoryId: -1,
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
        "results": [
            {
                "id": 18,
                "name": "Deuxième produit",
                "ean": "123456431334545",
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "perished_warehouse_quantity": 500,
                "closest_perishment_date": "2021-03-17",
                "perished_client_quantity": 100,
                "closest_client_date": "2021-03-17"
            }
        ],
        "count": 1
    }
}
```

This endpoints allows you to search for specific products in perishment dashboard

### HTTP Request

`GET search-supermarketPerishments`

### Query Parameters

| Parameters | Type   | Description                                                  |
| ---------- | ------ | ------------------------------------------------------------ |
| search     | String | Search query string. Minimum 3 chars or you'll get an INVALIDARGUMENT error |
| categoryId | Int    | The selected perishment screen filter                        |
| offset     | Int    | Offset applied to the query for pagination                   |

