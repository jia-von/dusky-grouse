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

