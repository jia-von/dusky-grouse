## Azure Retail Prices API Review
Based on the REST API, `https://prices.azure.com/api/retail/prices` provided, the general [API response structure](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices#api-response-examples) are shown below:

### First _layer_ JSON object
The first layer consists of six objects as shown below.
```json
{
  "BillingCurrency": "USD",
  "CustomerEntityId": "Default",
  "CustomerEntityType": "Retail",
  "Items": [...]
  "NextPageLink": "https://prices.azure.com:443/api/retail/prices?$skip=100",
  "Count": 100
}
```

The table below are created for my own learning purposes and the full description can be found in [Azure REST API property details](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices#api-property-details). The descriptions of each fields above are described below:

| Fields | Example Values | Definition |
| --- | --- | --- |
| `BillingCurrency` | `USD` | The currency that Microsoft currently uses to price all Azure services and USD currency are Microsoft retail prices. |
| `CustomerEntityId` | `Default` | No references can be found, it was assumed that `Default` was used generic way to show public retail prices. |
| `CustomerEntityType` | `Retail` | No references can be found, assume that `Retail` is used as a generic way to show the most likely customers using this API. |
| `Items` | `[array of objects]` | `Items` consists an array of objects which contains detailed information about Azure services and products. |
| `NextPageLink` | `https://prices.azure.com:443/api/retail/prices?$skip=100` | The API response provides [pagination](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices#api-response-pagination). At the end of the API response, it has the link to next page |
| `Count` | `100` |  For each API request, a maximum of [100 records](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices#api-response-pagination) are returned. |

### Second _layer_ JSON object
The JSON responded with an array of object nested within `Items`. Below shows an example of a typical JSON object properties:
```json
{
      "currencyCode": "USD",
      "tierMinimumUnits": 0,
      "retailPrice": 0.236555,
      "unitPrice": 0.236555,
      "armRegionName": "southindia",
      "location": "IN South",
      "effectiveStartDate": "2022-04-01T00:00:00Z",
      "meterId": "000009d0-057f-5f2b-b7e9-9e26add324a8",
      "meterName": "D14/DS14 Spot",
      "productId": "DZH318Z0BPVW",
      "skuId": "DZH318Z0BPVW/00QZ",
      "productName": "Virtual Machines D Series Windows",
      "skuName": "D14 Spot",
      "serviceName": "Virtual Machines",
      "serviceId": "DZH313Z7MMC8",
      "serviceFamily": "Compute",
      "unitOfMeasure": "1 Hour",
      "type": "DevTestConsumption",
      "isPrimaryMeterRegion": true,
      "armSkuName": "Standard_D14"
 }
```
The table below are created for my own learning purposes and the full description can be found in [Azure REST API property details](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices#api-property-details). The descriptions of each fields above are described below:
