# DataFactory Oracle-Cloud demo

- Simply adding these files into the same directory structure of an Azure DevOps or Github repository will import the pipeline and corresponding data-flows into Azure Data Factory.

- Linked Services in ADF will need to be recreated with same names as used in the pipelines (or renamed within DataFactory):
	* Blob Storage
	* SQL Server 
	* REST Service

- Security
	* Password for Oracle Cloud is currently hard-coded as plain text in the REST Service (along with rest Service URLs) and in its corresponding data set, this will need to be in a Azure-key-vault
		* OC_CurrenciesLOV.json:
			```json
				"parameters": {
					"baseURL": {
						"type": "string",
						"defaultValue": "https://CHANGE_THIS_TO_ACTUAL_URL.oraclecloud.com"
					},
					"serviceVersion": {
						"type": "string",
						"defaultValue": "11.13.18.05"
					},
					"serviceBase": {
						"type": "string",
						"defaultValue": "fscmRestApi"
					},
					"serviceName": {
						"type": "string",
						"defaultValue": "currenciesLOV"
					},
					"onlyData": {
						"type": "string",
						"defaultValue": "true"
					},
					"userName": {
						"type": "string",
						"defaultValue": "ENTER_USER_NAME_HERE_FOR_ORACLE_CLOUD_ACCESS"
					},
					"passWord": {
						"type": "string",
						"defaultValue": "ENTER_PASSWORD_HERE_FOR_ORACLE_CLOUD_ACCESS"
					}
				},		
			```
		* oc__DS_OC_CurrenciesLOV.json:
		```json
			"parameters": {
				"baseURL": "https://CHANGE_THIS_TO_ACTUAL_URL.oraclecloud.com",
				"serviceVersion": "11.13.18.05",
				"serviceBase": "@dataset().serviceBase",
				"serviceName": "@dataset().serviceName",
				"onlyData": "true",
				"userName": "ENTER_USER_NAME_HERE_FOR_ORACLE_CLOUD_ACCESS",
				"passWord": "ENTER_PASSWORD_HERE_FOR_ORACLE_CLOUD_ACCESS"
			}		
		```

- SQL Schema - below is the schema of the Currency Table that the data is loaded into, and will need to be created in advance

```SQL
create table currencyLOV (
    CurrencyCode char(3),
	CurrencyName varchar(100),
	Description varchar(500),
	EnabledFlag char(5),
	StartDateActive datetime,
	EndDateActive datetime,
	ActiveDate datetime,
	CurrencyFormat varchar(100),
	CurrencyFormatWithSymbol varchar(100),
	CurrencyFormatWithCode varchar(100),
	IssuingTerritoryCode char(20),
	Symbol char(10),
	Precision int,
    ExtendedPrecision int,
	CurrencyFlag char(5)
	)

```