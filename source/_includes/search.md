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
        "products": [
            {
                "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
                "name": "Produit test",
                "ean": "123456431334545",
                "id": 17,
                "stock_quantity": 9550
            }
        ],
        "search_count": 1
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

