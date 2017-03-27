# Definitions
<aside class="notice">The Definitions section defines the objects that are used throughout the REST API</aside>

## Book
Parameter | Type | Description
--------- | ------- | -----------
order_book | string | Name of Order Book
price | string | Price per unit
amount | string  | Amount in order
created_date | string | Date of Book creation in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
queue_position | string | How close this particular book is to the top of the queue

## Business Hours
Parameter | Type | Description
--------- | ------- | -----------
start_time | string | Start time in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) datetime format
end_time | string | End time in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) datetime format

## Company
Parameter | Type | Description
--------- | ------- | -----------
name | string | The name of the Company
code | string | Code used by exchange to identify Company
payment methods | array | Array of [Payment Methods](#payment-method) available 
country | string | Country of Company in [ISO 3166-1 Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) format
processing_hours | array | The processing hours for the Company.  This is an array of 7 [Business Hours](#business-hours) entries.  Each entry represents a day of the week starting with Sunday.
deposit_fee | string | Fee applied by Company for deposit
withdraw_fee | string |Fee applied by Company for withdraw
exchange_deposit_fee | string | Fee applied by Exchange for using Company to deposit
exchange_withdraw_fee | string | Fee applied by Exchange for using Company to withdraw
branding | string | If brand is different from Company
notes | string | Specific notes on how to deposit at Company
account_types | string | [Account types](#account-type) available at Company
account_validation_regex | string | Regex used for account validation
holidays | array | Array of Holiday Dates in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
        
## Document
Parameter | Type | Description
--------- | ------- | -----------
name | string | Name of Document
document_type | string | [Type of Document](#document-type)
file_type | string | Options available are `.jpeg`, `pdf` or `png`
file | string | Base64 encoded binary file
document_id_number | string | Identifier number of Document
name_on_doc | string | Name on document
expiration_date | string | Expiration date in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format

## Error Object
Parameter | Type | Description
--------- | ------- | -----------
error_code | string | Error Code
error_message | string | Error Message

## Exchange Account
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
company | string | Company Code
fiat_balance | string | Fiat balance amount
btc_balance | string | Bitcoin balance amount
fiat_account_number | string | Account number that receives Fiat amounts during Abra Deposit flow
fiat_account_type | string | [Type of Account](#account-type)
created_date | string | Created date of Exchange Account in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
modified_date | string | Date of last modification of Exchange Account in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format

## Field
Parameter | Type | Description
--------- | ------- | -----------
field_name| string | Field Name
field_type| string | [Type of Data] (#field-type) expected in Field
field_required | boolean | Boolean that defines if Field is mandatory - `true` or optional - `false`
field_validation_regex | string | Regex used for Field validation

## KYC
Parameter | Type | Description
--------- | ------- | -----------
kyc_level | string | Level of clearance kyc information provides
tx_limit | string | Transaction amount limit
daily_limit | string | Transaction limit for the day
weekly_limit | string | Transaction limit for the week
monthly_limit | string | Transaction limit for the month
dob | string | Date of birth in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
address | string | Address
address_2 | string | Additional address information
city | string | City
state_province | string | State or Province
postal_code | string | Postal code
country | string | Country in [ISO 3166-1 Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) format
documents | array | Array containing [Document](#document) objects
ssn_itin | string | Social Security or Individual Tax Identification Number

## Limit Order
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

## Location
Parameter | Type | Description
--------- | ------- | -----------
name | string | The name of the Location
code | string | Code used by Exchange to identify Location
payment methods | Array | Array of [Payment Methods](#payment-method) available 
address | string | Address
address_2 | string | Additional address information
city | string | City
state_province | string | State or Province
postal_code | string | Postal code
country | string | Country of Location in [ISO 3166-1 Alpha-2](https://en.wikipedia.org/wiki/ISO_3166-1_alpha-2#Officially_assigned_code_elements) format
gps_coordinates | string | GPS Coordinates of Location
phone_number | string | Phone Number
business_hours | array | Location's hours of operation.  This is an array of 7 [Business Hours](#business-hours) entries.  Each entry represents a day of the week starting with Sunday.
branding | string | If brand is different from Location
notes | string | Specific notes on how to Deposit at Location

## New Account
Parameter | Type | Description
--------- | ------- | -----------
first_name | string | First name
middle_name | string | Middle name
last_name | string | Last name
second_last_name | string | Second Last Name
email | string | Email
phone_number | string | Phone number
mfa_input | string | Multi-Factor Authentication field

## Notification
Parameter | Type | Description
--------- | ------- | -----------
notification_id | string | Identifier number for Notification
notification_type | string | [Type of Notification](#notification-type)
identifier | string | Depending on notification_type, this is the Identifier of either Account, Limit Order or Transaction
new_status | string | Depending on notification_type, this is either an [Account_status](#account-status), [Limit Order Status](#limit-order-status) or a [Transaction Status](#transaction-status)
message | string | Message regarding new status

## Order Book
Parameter | Type | Description
--------- | ------- | -----------
asks | array | An array of [Books](#book)
bids | array | An array of [Books](#book)

## Quote
Parameter | Type | Description
--------- | ------- | -----------
btc_amount | string | Amount of BTC to be exchanged
fiat_amount | string |  Amount of Fiat to be received
expiration_date_time | string | Date and Time of quote expiration in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format

## Transaction
Parameter | Type | Description
--------- | ------- | -----------
tx_id | string | Transaction Identifier.
tx_id_type | string | [Transaction ID type](#transaction-id-type) 
transaction_type | string |[Type of Transaction](#transaction-type)
partner_tx_id | string | Transaction Identifier in partner system
account_id | string | ID of Exchange Account
company_code | string | Company Code
location_code | string | Location Code
status | string | [Status of Transaction](#transaction-status)
currency | string | Currency of transaction in [ISO 4217](https://en.wikipedia.org/wiki/ISO_4217) format
amount | string | Amount of transaction
created_date | string | Created date of transaction in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
modified_date | string | Date of last modification on transaction in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format
destination_btc_address | string | Destination BTC Address
originating_btc_address | string | Originating BTC Address
fiat_account_number | string | In case of Fiat withdraw, this is the destination account number
fiat_account_type | string | [Account Type](#account-type) of destination Fiat account
related_transaction | string | UID of related transactions.  For example if the Transaction is Buy BTC, we might have here the UID of the Deposit Fiat transaction.  If Transaction is Buy Fiat, we would have the UID of the Deposit BTC transaction.
limit_order_id | string | ID of limit order



# Constants

## Account Status
Code | Account Status 
---- | ------- 
001 | Pending
002 | KYC Required
003 | MFA Required
004 | Active
005 | Closed
006 | Do you want to build a snowman?
007 | Rejected

## Account Type
Code | Account Type 
---- | ------- 
001 | Checking
002 | Savings
003 | Checking and Savings

## Document Type
Code | Document Type 
---- | ------- 
001 | Driver's License
002 | Passport
003 | ID Card
004 | Military ID
005 | Proof of Address
006 | Selfie

## Field Type
Code | Field Type 
---- | ------- 
001 | string
002 | int
003 | boolean
004 | file

## Limit Order Status
Code | Limit Order Type 
---- | ------- 
001 | open
002 | processing
003 | fulfilled

## Notification Type
Code | Notification Type
---- | -------
001 | Account
002 | Limit Order
003 | Transaction

## Payment Method
Code | Payment Method
---- | -------
001 | Bank Account
002 | Cash
003 | ATM

## Transaction ID Type
Code | Transaction ID Type
---- | -------
001 | Number
002 | Barcode
003 | QR Code
004 | SMS Pin

## Transaction Status
Code | Transaction Status
---- | -------
001 | Initiated
002 | Completed
003 | On Hold
004 | Done
005 | Cancelled

## Transaction Type
Code | Transaction Type 
---- | -------
001 | Deposit Fiat
002 | Buy BTC
003 | Withdraw BTC
004 | Deposit BTC
005 | Sell BTC
006 | Withdraw Fiat