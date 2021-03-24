# Alley Manager

## Get Product Infos

```javascript
let cloudFunction = firebase.app().functions('europe-west3').httpsCallable('alleyManager-getProductInfos');
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
    "result": [
        {
            "image_url": "https://mysterytea-website.s3.eu-west-3.amazonaws.com/assets/2020/08/21181335/NEW-FraiseFleurieDoypack.png",
            "selling_unit": "2x12l",
            "legal_unit_price": "18.54€/kg",
            "price": 29.9,
            "name": "Deuxième produi",
            "brand": "MysteryTea",
            "promotion_type": null,
            "bought_quantity": null,
            "paid_quantity": null,
            "discount_value": null,
            "total_Stock": 92
        }
    ]
}
```

This endpoint fetches product infos on tag from alley manager

### HTTP Request

`GET alleyManager-getProductInfos`

### Query Parameters

| Parameter | Type | Description          |
| --------- | ---- | -------------------- |
| productId | Int  | Scanned product's id |

