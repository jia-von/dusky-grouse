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
The [Azure virtual machine (VM) nomenclature](https://docs.microsoft.com/en-us/azure/virtual-machines/vm-naming-conventions) will be used to _translate_ VM names into descriptions such as storage, vCPU, and memory.

#### Description
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

#### Example
**M8-2ms_v2**
| Value | Description |
| --- | --- |
| Family | M |
| # of vCPUs | 8 |
| # of constrained (actual) vCPUs | 2 |
| Additive Features |  	m = memory intensive, s = Premium Storage capable |
| Version | v2 |

**NC4as_T4_v3**
| Value | Description |
| --- | --- |
| Family | N |
| Sub-family | C |
| # of vCPUs | 4 |
| Additive Features | a = AMD-based processor , s = Premium Storage capable |
| Accelerator Type | T4 |
| Version | v3 |

#### Observation
Based on the examples above, it is noted that:
- **Accelerator Type** has an underscore appended before the model, **\_T4** and it will be appended by an underscore after because it occupies the second last column before **Version**.
- **Version** occupies the last column and will have an underscore appendded before the model, **\_v3**.
- **# constrained vCPU** which is pressumed to be an optional feature for VM has a dash appended before an integer, **-2**.
- **Additive Features** is noted to come in lower case alphabet pairs, **as** or **ms**.

## Azure Virtual Machine Calculator Review
According to [Azure VM Pricing](https://azure.microsoft.com/en-us/pricing/details/virtual-machines/linux/) page, there a few inputs a potential customer need to use to decide on the VM pricing:

### OS/Software
**Linux**
- CentOS or Ubuntu Linux
- Red Hat Enterprise Linux
- Red Hat Enterprise Linux with HA
- RHEL for SAP with HA
- RHEL for SAP Business Applications
- SUSE Linux Enterprise + Patching Only
- SUSE Linux Enterprise + 24x7 Support
- SUSE Linux Enterprise for HPC + 24x7 Support
- SUSE Linux Enterprise for SAP Application + 24x7 Support
- Ubuntu Advantage Essential
- Ubuntu Advantage Standard
- Ubuntu Advantage Advanced
- Machine Learning Server on Red Hat Enterprise Linux
- Machine Learning Server on Ubuntu or Centos Linux
- SQL Server Enterprise Ubuntu Linux
- SQL Server Standard Ubuntu Linux
- SQL Server Web Ubuntu Linux
- SQL Server Enterprise Red Hat Linux
- SQL Server Standard Red Hat Linux
- SQL Server Web Red Hat Linux
- SQL Server Enterprise SUSE Priority
- SQL Server Standard SUSE Priority
- SQL Server Web SUSE Priority
- SQL Server Enterprise Ubuntu Pro
- SQL Server Standard Ubuntu Pro
- SQL Server Web Ubuntu Pro
- SQL Server Developer Ubuntu Pro

**Windows**
- Windows OS
- BizTalk Enterprise
- BizTalk Standard
- Machine Learning Server
- Share Point
- SQL Server Enterprise
- SQL Server Standard
- SQL Server Web

### Category
- [ ] General purpose
- [ ] Compute optimized
- [ ] Memory optimized
- [ ] Storage optimized
- [ ] GPU
- [ ] High performance compute

### Others
Other inputs considered in VM pricing options:
- [x] VM Series
- [x] Region
- [x] Currency
- [ ] Display pricing by: _month_ or _hour_
- [x] [Spot](https://azure.microsoft.com/en-ca/services/virtual-machines/spot/)
- [x] [Low-priority](https://azure.microsoft.com/en-ca/blog/low-priority-scale-sets/)
- [x] vCPUs
- [x] Constrained vCPUs
