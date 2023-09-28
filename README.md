# Getting Started

## Running Locally

Make sure you have [Node.js](http://nodejs.org/) and the [Heroku CLI](https://cli.heroku.com/) installed.

```sh
$ git clone https://github.com/forcedotcom/commerce-heroku-sample-response.git # or clone your own fork
$ cd commerce-heroku-sample-response
$ npm install
$ npm start
```

Your app should now be running on [localhost:5000](http://localhost:5000/).

## Deploying to Heroku

```
$ heroku create
$ git push heroku master
$ heroku open
```
or

[![Deploy to Heroku](https://www.herokucdn.com/deploy/button.png)](https://heroku.com/deploy)

## API Endpoints

### Home
- Endpoint: `/`
- Response: Renders an index page.

### Calculate Shipping Rates
- Endpoint: `/calculate-shipping-rates`
- Method: GET
- Response: Returns the shipping rates in JSON format.


- Response Example:
```json
{
    "status": "calculated",
    "rate": {
        "serviceName": "Canada Post",
        "serviceCode": "SNC9600",
        "shipmentCost": 11.99,
        "otherCost": 5.99
    }
}
```

### Calculate Shipping Rates with Language
- Endpoint: `/calculate-shipping-rates-with-lang`
- Method: GET
- Query Parameters:
    - `lang`: Language code (`en`, `de`, or `ja`).
- Response: Returns the shipping rates in the specified language in JSON format.


- Request Example: `/calculate-shipping-rates-with-lang?lang=de`
- Response Example (for "de"):
```json
{
  "status": "calculated",
  "rate": {
    "serviceName": "Kanada Post",
    "serviceCode": "SNC9600",
    "shipmentCost": 11.99,
    "otherCost": 5.99
  }
}
```

### Calculate Shipping Rates (Winter 21)
- Endpoint: `/calculate-shipping-rates-winter-21`
- Method: GET
- Response: Returns two shipping rates in JSON format.


- Response Example:
```json
[
  {
    "status": "calculated",
    "rate": {
      "name": "Delivery Method 1",
      "serviceName": "Test Carrier 1",
      "serviceCode": "SNC9600",
      "shipmentCost": 11.99,
      "otherCost": 5.99
    }
  },
  {
    "status": "calculated",
    "rate": {
      "name": "Delivery Method 2",
      "serviceName": "Test Carrier 2",
      "serviceCode": "SNC9600",
      "shipmentCost": 15.99,
      "otherCost": 6.99
    }
  }
]
```

### Calculate Shipping Rates (Winter 21) with Language
- Endpoint: `/calculate-shipping-rates-winter-21-with-lang`
- Method: GET
- Query Parameters:
    - `lang`: Language code (`en`, `de`, or `ja`).
- Response: Returns two shipping rates in the specified language in JSON format.


- Request example: `/calculate-shipping-rates-winter-21-with-lang?lang=ja`
- Response Example:
```json
[
  {
    "status": "calculated",
    "rate": {
      "name": "Delivery Method 1",
      "serviceName": "Test Carrier 1",
      "serviceCode": "SNC9600",
      "shipmentCost": 11.99,
      "otherCost": 5.99
    }
  },
  {
    "status": "calculated",
    "rate": {
      "name": "Delivery Method 2",
      "serviceName": "Test Carrier 2",
      "serviceCode": "SNC9600",
      "shipmentCost": 15.99,
      "otherCost": 6.99
    }
  }
]
```

### Get Inventory
- Endpoint: `/get-inventory`
- Method: GET
- Query Parameters:
    - `skus`: A comma-separated list of SKUs.
- Response: Returns inventory quantities for the provided SKUs in JSON format.


- Request Example: `/get-inventory?skus="SKU123","SKU456"`
- Response Example:
```json
{
    "SKU123": 9999,
    "SKU456": 9999
}
```

### Get Sales Prices
- Endpoint: `/get-sales-prices`
- Method: GET
- Query Parameters:
    - `skus`: A comma-separated list of SKUs.
- Response: Returns sales prices for the provided SKUs in JSON format.


- Request Example:`GET /get-sales-prices?skus="SKU123","SKU_FOR_TEST"`
- Response Example:
```json
{
    "SKU123": 0.00,
    "SKU_FOR_TEST": 100.00
}
```

### Get Tax Rates
- Endpoint: `/get-tax-rates`
- Method: GET
- Query Parameters:
    - `amountsBySKU`: A comma-separated list of SKU and amount pairs.
- Response: Returns tax rates for the provided amounts by SKU in JSON format.


- Request Example: `/get-tax-rates?amountsBySKU="SKU123":100.00,"SKU456":200.00`
- Response Example:
```json
{
    "SKU123": {
        "taxAmount": 8.00,
        "taxRate": 0.08,
        "taxName": "GST"
    },
    "SKU456": {
        "taxAmount": 16.00,
        "taxRate": 0.08,
        "taxName": "GST"
    }
}
```

### Get Sales Prices (POST method)
- Endpoint: `/get-sales-prices`
- Method: POST
- Request Body:
    - `skus`: List of SKUs.
- Response: Returns sales prices for the provided SKUs in JSON format.


- Request Example: 
```json
{
    "skus": ["SKU123", "SKU_FOR_TEST"]
}
```
- Response Example:
```json
{
  "SKU123": 0.00,
  "SKU_FOR_TEST": 100.00
}
```

### Get Tax Rates by Tax Type
- Endpoint: `/get-tax-rates-by-tax-type`
- Method: GET
- Query Parameters:
    - `amountsBySKU`: Amounts by SKU.
    - `taxType`: Tax type (e.g., `Gross`).
- Response: Returns tax rates for the provided amounts by SKU and tax type in JSON format.


- Request Example: `/get-tax-rates-by-tax-type?amountsBySKU="SKU123":100.00,"SKU456":200.00&taxType=Gross`
- Response Example:
```json
{
    "SKU123": {
        "taxAmount": 7.41,
        "taxRate": 0.08,
        "taxName": "GST"
    },
    "SKU456": {
        "taxAmount": 14.81,
        "taxRate": 0.08,
        "taxName": "GST"
    }
}
```

### Get Tax Rates with Adjustments
- Endpoint: `/get-tax-rates-with-adjustments`
- Method: GET
- Query Parameters:
    - `amountsBySKU`: Amounts by SKU.
    - `country`: Country code.
    - `state`: State code.
    - `taxType`: Tax type (e.g., `Gross`).
- Response: Returns tax rates with adjustments for the provided parameters in JSON format.


- Request Example: `/get-tax-rates-with-adjustments?amountsBySKU={"SKU123":{"cartItemId":"1","amount":100.00}}&country=US&state=CA&taxType=Gross`
- Response Example:
```json
{
  "SKU123": {
    "cartItemId": "1",
    "taxAmount": 7.41,
    "adjustmentTaxAmount": 0.00,
    "itemizedPromotionTaxAmounts": [],
    "totalItemizedPromotionTaxAmount": 0.00,
    "grossUnitPrice": 100.00,
    "netUnitPrice": 92.59,
    "taxRate": 0.08,
    "taxName": "GST"
  }
}
```

### Get Tax Rates with Adjustments (POST method)
- Endpoint: `/get-tax-rates-with-adjustments-post`
- Method: POST
- Request Body:
    - `amountsBySKU`: Amounts by SKU.
    - `country`: Country code.
    - `state`: State code.
    - `taxType`: Tax type (e.g., `Gross`).
- Response: Returns tax rates with adjustments for the provided parameters in JSON format.


- Request Example:
```json
{
    "amountsBySKU": {
        "SKU123": {
            "Id": "1",
            "TotalLineAmount": 100.00,
            "AdjustmentAmount": 0.00,
            "Quantity": 1,
            "CartItemPriceAdjustments": {
                "records": []
            }
        }
    },
    "country": "US",
    "state": "CA",
    "taxType": "Gross"
}
```
- Response Example:
```json
{
  "1": {
    "cartItemId": "1",
    "taxAmount": 7.41,
    "adjustmentTaxAmount": 0.00,
    "itemizedPromotionTaxAmounts": [],
    "totalItemizedPromotionTaxAmount": 0.00,
    "grossUnitPrice": 100.00,
    "netUnitPrice": 92.59,
    "taxRate": 0.08,
    "taxName": "GST"
  }
}
```

