## Azure Retail Prices API Review
Based on the REST API, `https://prices.azure.com/api/retail/prices` provided, the general [API response structure](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices#api-response-examples) are shown below:

### First _layer_ json object
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
