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

| Fields | Example Values | Definition |
| --- | --- | --- |
| `currencyCode` | `USD` | The currency in which rates are defined and returns prices in USD unless specified. |
| `tierMinimumUnits` | `0` | Minimum units of consumption to [avail](https://www.oxfordlearnersdictionaries.com/definition/english/avail_2) the price. |
| `reservationTerm` | `1 Year` | Reservation term – one year or three years |
| `retailPrice` | `0.236555` | Prices without discount |
| `unitPrice` | `0.236555` |  |
| `armRegionName` | `southindia` | [ARM](https://docs.microsoft.com/en-us/azure/azure-resource-manager/management/overview) region where the service is available. This version only supports prices on Commercial Cloud. |
| `location` | `IN South` | Azure data center where the resource is deployed. |
| `effectiveStartDate` | `2022-04-01T00:00:00Z` | Optional field. Shows the date when the retail prices are effective |
| `effectiveEndDate` | `2022-04-01T00:00:00Z` | Not all Azure services provides ends date, but the API does have `effectiveEndDate` reponse where available. |
| `meterId` | `000009d0-057f-5f2b-b7e9-9e26add324a8` | Unique identifier of the resource. Further information about [Meter ID](https://azure.microsoft.com/en-us/updates/update-reports-for-improved-meter-names/). _For Compute Hour usage, there is a meter for each VM Size + OS (Windows, Non-Windows) + Region._, _For Premium software usage, there is a meter for each software type. Most premium software images have different meters for each core size._ |
| `meterName` | `D14/DS14 Spot` | Name of the meter. |
| `productId` | `DZH318Z0BPVW` | UniqueID of the product. |
| `skuId` | `DZH318Z0BPVW/00QZ` | UniqueID for the SKU |
| `productName` | `Virtual Machines D Series Windows` | Product name. |
| `skuName` | `D14 Spot` | SKU name |
| `serviceName` | `Virtual Machines` | Name of the service |
| `serviceId` | `DZH313Z7MMC8` | UniqueID of the service |
| `serviceFamily` | `Compute` | Service family of the SKU |
| `unitOfMeasure` | `1 Hour` |  	How usage is measured for the service. |
| `type` | `DevTestConsumption` |  [DevTestConsumption](https://azure.microsoft.com/en-ca/pricing/dev-test/) meter consumption type. Other types are _[Reservation](https://azure.microsoft.com/en-ca/reservations/)_, _[Consumption](https://azure.microsoft.com/en-gb/pricing/details/functions/)_. |
| `isPrimaryMeterRegion` | `true` | Indicates whether the [meter region](https://docs.microsoft.com/en-us/azure/virtual-machines/vm-usage) is set as a primary meter or not. Primary meters are used for charges and billing. |

### Nomenclature Breakdown
The Azure virtual machine (VM) nomenclature will be used to _translate_ VM names into descriptions such as storage, vCPU, and memory.

#### Example
**[Family]** + _[Sub-family]*_ + **[# of vCPUs]** + _[Constrained vCPUs]*_ + **[Additive Features]** + _[Accelerator Type]*_ + **[Version]**

| Value | Description |
| --- | --- |
| Family | Indicates the VM Family Series |
| \*Sub-family | Used for specialized VM differentiations only |
| # of vCPUs | Denotes the number of vCPUs of the VM |
| \*Constrained vCPUs | Used for certain VM sizes only. Denotes the number of vCPUs for the [constrained vCPU capable size](https://docs.microsoft.com/en-us/azure/virtual-machines/constrained-vcpu) |
| Additive Features | One or more lower case letters denote additive features, such as:
|  | a = AMD-based processor |
|  | d = diskfull (local temp disk is present); this is for newer Azure VMs, see Ddv4 and Ddsv4-series |
|  | i = isolated size |
|  | l = low memory; a lower amount of memory than the memory intensive size |
|  | m = memory intensive; the most amount of memory in a particular size |
|  | t = tiny memory; the smallest amount of memory in a particular size |
|  | s = Premium Storage capable, including possible use of Ultra SSD (Note: some newer sizes without the attribute of s can still support Premium Storage e.g. M128, M64, etc.) |
| \*Accelerator Type | Denotes the type of hardware accelerator in the specialized/GPU SKUs. Only the new specialized/GPU SKUs launched from Q3 2020 will have the hardware accelerator in the name. |
| Version | Denotes the version of the VM Family Series |

