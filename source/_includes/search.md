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

