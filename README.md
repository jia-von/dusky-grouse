# Introduction  
I want this to be a fun project to practice my problem solving skills in the context of extract, load, and transform (ETL) data using Apache Hop. Just spending my free time being curious about software in general. Poking things around is what I like to do. 

## Objective
The objective of this exercise is to _translate_ Azure VM SKU product name into categories understandable by humans to reflect what [Azure Pricing Calculator](https://azure.microsoft.com/en-us/pricing/calculator/) in VM services uses as input.

## Prerequisites
- [Apache Hop 1.2.0](https://hop.apache.org/download/)
- [Azure Retail Prices REST API ](https://docs.microsoft.com/en-us/rest/api/cost-management/retail-prices/azure-retail-prices)

## Instructions
Below are the list of instructions on how to use this pipeline.

### Clone
You may clone the repository using the command below within Git Bash Terminal for HTTPS URL. Alternatively, you may use SSH URL or ZIP file as provided by GitHub.
```console
git clone https://github.com/jia-von/dusky-grouse.git
```

Clone the `dusky-grouse` project into the Hop projects folder as shown below:
```
hop/
  |-- config/
    |-- metadata/
    |-- projects/
      |--default/
      |--dusky-grouse/ <-- clone directory here.
      |--samples/
    |--hop-config.json
```
