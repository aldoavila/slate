# Definitions
<aside class="notice">The Definitions section defines the objects that are used throughout the REST API</aside>



## Bank
Parameter | Type | Description
--------- | ------- | -----------
bank_name | string | The name of the Bank
bank_code | string | Code used by exchange to identify Bank
country | string | Country of Bank in [ISO 3166-1 Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements)
processing_hours | array | The processing hours for the Bank.  This is an array of 7 [Business Hours](#business-hours) entries.  Each entry represents a day of the week starting with Sunday.
branch_hours | array |The processing hours for the Bank.  This is an array of 7 [Business Hours](#business-hours) entries.  Each entry represents a day of the week starting with Sunday.
online_banking | boolean | Boolean that states if online banking is available
deposit_fee | string | Fee applied by bank for deposit
withdrawal_fee | string |Fee applied by bank for withdrawal
account_types | string | [Account types](#account-type) available at bank
account_validation_regex | string | Regex used for account validation

##Book
Parameter | Type | Description
--------- | ------- | -----------
order_book | string | Name of Order Book
price | string | Price per unit
amount | string  | Amount in order
created_date | string | Date of Book creation in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format

##Business Hours
Parameter | Type | Description
--------- | ------- | -----------
start_time | string | Start time in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) datetime format
end_time | string | End time in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) datetime format

##Document
Parameter | Type | Description
--------- | ------- | -----------
name | string | "Name of Document"
document_type | string | [Type of Document](#document-type)
file_type | string | Options available are `.jpeg`, `pdf` or `png`
file | string | Base64 encoded binary file
document_id_number | string | Identifier number of Document
name_on_doc | string | Name on document

##Error Object
Parameter | Type | Description
--------- | ------- | -----------
error_code | string | Error Code
error_message | string | Error Message

##Exchange Account
Parameter | Type | Description
--------- | ------- | -----------
id | string | Account ID at Exchange
api_key | string | API key for user
account_status | string | [Account Status](#account-status)
status_message | string | Message explaining reason for current Status
status_modified_date | string | Date of last status modification in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
first_name | string | First name
middle_name | string | Middle name
last_name | string | Last name
second_last_name | string | Second last name
email | string | Email
phone_number | string | Phone number
kyc | object | [KYC](#kyc)
bank | string | Bank Code
fiat_balance | string | Fiat balance amount
btc_balance | string | Bitcoin balance amount
fiat_account_number | string | Account number that receives Fiat amounts during Abra Deposit flow"
fiat_account_type | string | [Type of Account](#account-type)
created_date | string | Created date of Exchange Account in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
modified_date | string | Date of last modification of Exchange Account in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format

##Field
Parameter | Type | Description
--------- | ------- | -----------
field_name| string | Field Name
field_type| string | [Type of Data] (#field-type) expected in Field
field_requirement | boolean | Boolean that defines if field is mandatory - `true` or optional - `false`
field_validation_regex | string | Regex used for field validation
        
##KYC
Parameter | Type | Description
--------- | ------- | -----------
kyc_level | string | Level of clearance kyc information provides
dob | string | Date of birth in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
address | string | Address
address_2 | string | Additional address information
city | string | City
state_province | string | State or Province
postal_code | string | Postal code
country | string | Country in [ISO 3166-1 Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements)
documents | array | Array containing [Document](#document) objects
ssn_itin | string | Social Security or Individual Tax Identification Number

##Limit Order
Parameter | Type | Description
--------- | ------- | -----------
order_id | string | Unique identifier of order
created_date | string | Created date of Limit Order in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
modified_date | string | Date of last modification of Limit Order in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
order_type | string | Type of Order.  Options are `BUY` or `SELL`
currency | string | Order Currency in [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) format
amount | string | Amount of currency
price | string | Price per Unit
unfilled_amount | string | Amount of order that remains unfilled
status | string |[Status of limit order](#limit-order-status)

##New Account
Parameter | Type | Description
--------- | ------- | -----------
first_name | string | First name
middle_name | string | Middle name
last_name | string | Last name
second_last_name | string | Second Last Name
email | string | Email
phone_number | string | Phone number
mfa_input | string | Multi-Factor Authentication field

##OrderBook
Parameter | Type | Description
--------- | ------- | -----------
asks | array | An array of [Books](#book)
bids | array | An array of [Books](#book)

##Quote
Parameter | Type | Description
--------- | ------- | -----------
btc_amount | string | Amount of BTC to be exchanged
fiat_amount | string |  Amount of Fiat to be received
expiration_date_time | string | Date and Time of quote expiration in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format

##Transaction
Parameter | Type | Description
--------- | ------- | -----------
uid | string | Unique Identifier of Transaction
transaction_type | string |[Type of Transaction](#transaction-type)
status | string | [Status of Transaction](#transaction-status)
currency | string | Currency of transaction in [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) format
amount | string | Amount of transaction
created_date | string | Created date of transaction in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
modified_date | string | Date of last modification on transaction in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
destination_btc_address | string | Destination BTC Address
originating_btc_address | string | Originating BTC Address
related_transaction | string | UID of related transactions.  For example if the Transaction is Buy BTC, we might have here the UID of the Deposit Fiat transaction.  If Transaction is Buy Fiat, we would have the UID of the Deposit BTC transaction.
limit_order_id | string | ID of limit order

#Constants
##Account Status
Code | Field Type 
---- | ------- 
001 | Pending
002 | KYC Required
003 | MFA Required
004 | Active
005 | Closed
006 | Do you want to build a snowman?

## Account Type
Code | Account Type 
---- | ------- 
001 | Checkings
002 | Savings
003 | Checkings and Savings

##Document Type
Code | Field Type 
---- | ------- 
001 | Driver's License
002 |  Passport
003 | ID Card
004 | Military ID

##Field Type
Code | Field Type 
---- | ------- 
001 | string
002 | int
003 | boolean
004 | file

##Limit Order Status
Code | Field Type 
---- | ------- 
001 | open
002 | processing
003 | fulfilled

##Transaction Status
Code | Field Type
---- | -------
001 | Initiated
002 | Completed
003 | On Hold
004 | Done

##Transaction Type
Code | Field Type 
---- | -------
001 | Deposit Fiat
002 | Buy BTC
003 | Withdrawal BTC
004 | Deposit BTC
005 | Buy Fiat
006 | Withdrawal Fiat