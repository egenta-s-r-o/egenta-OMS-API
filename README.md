# EGENTA OMS API

# Version 0.1

### Documentation Version 0.1

### 22 April 2025

### The API is provided to allow connecting the EGENTA OMS to new marketplaces.

### When new market is integrated it first need to be set up on the OMS, (there are several options

### to be set. Depending on what is available from the marketplace and the available order data.

### We will provide list of questions in the next version of this document).

### Once the new marketplace is set on the OMS system, Egenta will provide you with the

### credentials to access the API.

### The order management process at the OMS requires several actions:

### 1. Order Import - Importing new orders to the OMS

### 2. Order Update - Updating the order as shipped at the market, once the order is shipped

### from Egenta.

### 3. Order Status - Getting orders status from the Marketplace

### Authentication

### For API version up to 1.0 - you can just authenticate by sending the key as part of the requests

### to the API. The IP address of the server running your App should also be white listed at Egenta.


## 1. Order Import

### Access point: https://oms.egenta.eu/api/order_import.php

### Method: PUT

### Data to be provided: JSON array with the following structure:

'secret_id' The App Credentials
'secret_key' The App Credentials
'orders' Array of orders to be imported.

### Example Data:

##### {

"secret_id": 1,
"secret_key": 1,
"orders": [
{
"OrderId": "SCID01210525142759UT8E",
"Status": "testCdiscount_04e",
"Reference": "custm_001",

"StoreId": "custm_001",
"StoreName": "custm_001",

"PurchaseDate": "yyyy-mm-dd HH:nn:ss",
"LastUpdateDate": "yyyy-mm-dd HH:nn:ss",
"EarliestShipDate": "yyyy-mm-dd HH:nn:ss",
"LatestShipDate": "yyyy-mm-dd HH:nn:ss",
"EarliestDeliveryDate": "2025- 04 - 22 10:26:05",
"LatestDeliveryDate": "2025- 04 - 22 10:26:05",

"SalesChannelId": "custm_001",
"SalesChannel": "custm_001",
"BusinessOrder": "1",
"BuyerName": "1",
"BuyerEmail": "1",
"OrderTotal": "100.00",
"CurrencyCode": "EUR",
"NumberOfItemsUnshipped": " 1 ",
"requested_shipping_carrier": "DHL",

"shippingAddress": {
"firstName": "Jane",
"lastName": "Doe",
"companyName": "Company Inc.",
"addressLine1": "12 avenue de paris",
"addressLine2": "Bat C",
"addressLine3": "4eme etage",
"postalCode": "75001",
"city": "Paris",


"stateOrRegion": "Ile de france",
"phone": "17373737373",
"countryCode": "FR"
},

"billingAddress": {
"firstName": "Jane",
"lastName": "Doe",
"companyName": "Company Inc.",
"addressLine1": "12 avenue de paris",
"addressLine2": "Bat C",
"addressLine3": "4eme etage",
"postalCode": "75001",
"city": "Paris",
"stateOrRegion": "Ile de france",
"phone": "17373737373",
"countryCode": "FR"
},

"lines": [
{
"productId": "ASIN",
"sellerProductId": "SKU",
"orderLineId": "line1",
"productTitle": "Product Name",
"quantity": 1,
"sellingPrice": 122,
"sellingPriceCurrencyCode": "EUR",
"shippingCost": 12,
"shippingCostCurrencyCode": "EUR",
"commission_amountWithVat": 12,
"commission_amountWithoutVat": 10

},

##### ]

##### }

##### ]

### }

### Fields description:

#### OrderId Required. The market order ID

#### Status Required. Current order status at market

#### Reference If there is order Reference ID or second Order ID for some

#### reason.

#### StoreId if available, some markets use it

#### StoreName if available, some markets use it

#### PurchaseDate Required. Format: "yyyy-mm-dd HH:nn:ss"


#### LastUpdateDate Required. Format: "yyyy-mm-dd HH:nn:ss"

#### EarliestShipDate Format: "yyyy-mm-dd HH:nn:ss"

#### LatestShipDate Required. Format: "yyyy-mm-dd HH:nn:ss"

#### EarliestDeliveryDate Format: "yyyy-mm-dd HH:nn:ss"

#### LatestDeliveryDate Format: "yyyy-mm-dd HH:nn:ss" Latest delivery date is

#### important, if available. If not we have set of specific rules. This is

#### part of the initial App setup at the OMS.

#### SalesChannelId

#### SalesChannel Required. If not available, use market name.

#### BusinessOrder Boolean.

#### BuyerName Required.

#### BuyerEmail

#### OrderTotal Required. Float.

#### CurrencyCode Required. Example: “EUR”. As part of the initial setup, let us

#### know, please if we are to expect different currencies.

#### NumberOfItemsUnshipped Just number of items in the order

#### requested_shipping_carrier If the customer provided any delivery instructions, requests,

#### shipping method preferred etc.

#### shippingAddress : Required.

#### firstName Required.

#### lastName Required.

#### companyName

#### addressLine1 Required.

#### addressLine

#### addressLine

#### postalCode Required.

#### city Required.

#### stateOrRegion Required.

#### phone

#### countryCode Required. Two digits country code. Example: "FR"

#### billingAddress : Required.

#### firstName Required.

#### lastName Required.

#### companyName

#### addressLine1 Required.

#### addressLine

#### addressLine

#### postalCode Required.

#### city Required.

#### stateOrRegion

#### Required.

#### phone


#### countryCode Required. Two digits country code. Example: "FR"

#### lines : Required

#### productId Required.

#### sellerProductId Required.

#### orderLineId Required.

#### productTitle Required.

#### quantity Required.

#### sellingPrice Required.

#### sellingPriceCurrencyCode Required.

#### shippingCost Required.

#### shippingCostCurrencyCode Required.

#### commission_amountWithVat Nice to have. If market provides it or if can be calculated.

#### commission_amountWithoutVat Nice to have. If market provides it or if can be calculated.


## 2. Order Update

### There are two steps to update an order as shipped at the market.

### First: You need to get list of orders that are already shipped by Egenta.

### Second: Once each order is updated at the market (from your App) you need to call the API

### again to update the order as shipped on the market (so it will be removed from the list of

### orders that you are getting at the previous step).

### Step1:

### Access point: https://oms.egenta.eu/api/order_update_get_list.php

### Method: GET

### Data to be provided - JSON array with the following structure:

'secret_id' The App Credentials
'secret_key' The App Credentials

### The response will be a JSON with the following fields for each order:

'orderId' This is the Market Order ID
'carrierName' Carrier name
'parcelNumber' Tracking Number

### Step2:

### Access point: https://oms.egenta.eu/api/order_update_update.php

### Method: POST

### Data to be provided - JSON array with the following structure:

'secret_id' The App Credentials
'secret_key' The App Credentials
'OrderID' The Market Order ID
'updated' Set to 1 (means order updated)


## 3. Order Status

### Egenta OMS need to check order status on the market regularly. The system creates list of

### order to be checked. Your App needs to:

### First: Get list of orders that need status check on the market (check regularly - few times in

### each hour).

### Second: Check each of the orders from list and send the current market status of the order back

### to the OMS.

### Step1:

### Access point: https://oms.egenta.eu/api/order_status_get_list.php

### Method: GET

### Data to be provided - JSON array with the following structure:

'secret_id' The App Credentials
'secret_key' The App Credentials

### The response will be a JSON with all orders to be checked:

'orderId' This is the Market Order ID

### Step2:

### Access point: https://oms.egenta.eu/api/order_status_update.php

### Method: POST

### Data to be provided - JSON array with the following structure:

'secret_id' The App Credentials
'secret_key' The App Credentials
'OrderID' The Market Order ID
'OrderStatusTranslated' The current order status, “translated” for
the OMS system (see below)

Order status for the OMS should be one of the following values:
"Shipped" , "Unshipped" or "Canceled".


### Each market has many different ways to call the order status at their end; your App should

### “translate” the real market status to one of those three that OMS work with.


