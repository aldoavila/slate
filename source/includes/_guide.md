# Introduction
This document is a guide for integrating with Abra.  The steps required as well 
as the proposed API that an exchange should build are defined below.  By building 
the API, an exchange will be compatible with the Abra Exchange Flow.  This means 
that integration  with Abra  will be faster, but, even more important, the user 
experience will continue to be excellent.  This is a direct result of updates 
allowing a user to, at any given moment, view their account or transaction status.

# Integration Steps
The steps defined below are those taken by Abra and the exchange to integrate
successfully.  Depending on the integration, more or less steps may be required.

1. **Introductory call -** Sometimes this call will just be the business sides, but it 
may also involve the tech teams. Tech contacts are exchanged at this point and
if available the Exchange API is shared.
2. **Abra reviews exchange API -** Abra reviews the API with the purpose of verifying
if API fits Abra' flow.  Any functionality that is missing will be noted.
3. **Tech Call -** Exchange and Abra tech teams have a conversation to define
missing functionality in API.  Exchange's customer communication is also
discussed.  Sandbox credentials are also shared at this point to help with
development and testing.
4. **Exchange Development -** The exchange implements the missing functionality as close 
as close as possible to the suggested solutions defined in this API guide.  Abra
is available for any questions.
5. **Abra Development -** If anything special is required by the exchange, Abra will
develop with the availability of the exchange in case questions arise.
6. **Pre-certification -** Exchange and Abra pre-certify the integration in the sandbox
environment.  Any failed cases are noted.
7. **Fix failure cases -** Abra and the Exchange fix any cases that failed during the
pre-certification
8. **Finish pre-certification -** Any cases that were failing are redone to confirm that 
they are now passing.
9. **Certify -** Run through certification steps.  At this point any failed steps require
the certification to be restarted from the beginning once a fix is implemented.
10. **Exchange Production Credentials -** The Exchange provides Abra with production 
credentials and production permissions.  Abra does the same.
11. **Deployment -** Code is deployed to production and as well as any configuration
changes
12. **Production Basic Tests -** A few basic tests are done in production to confirm 
deployment was a success.
13. **Activate -** Once the deployment has been confirmed as successful, and Business 
Dev. has given the ok, the Exchange is activated in Abra system in order to accept real 
requests.
14. **Monitor -** Thorough monitoring is done in the first few hours of activation to confirm 
integration is running smoothly.  Close monitoring is kept for at least 2 weeks. After that 
enough activity should have been experienced to ease fears of potential issues.  Integration 
now enters normal monitoring.



# Guide
There are four critical actions that an exchange has to provide.

- Account creation
- Deposit
- Withdraw
- Updates

This guide will explain how Abra intends to use the API in order to execute the actions 
listed above.  Each action defines the API use for the "Happy Path" as well as what each
call is doing.  They will also define which API's will be used during error cases and
why they are being called.

## Account Creation
Before a user can do withdraws and deposits, they need to have an account.  It's very
important that account creation can be handled via the API so that the entire process
can be automated.  It's also important that outside interaction is not required by the
user, as the user will not necessarily know they are dealing with an exchange.  What
this means is that email verification or two-factor authentication should not be required
as Abra is already verifying that it's the user sending the request.

A user first selects what type of endpoint they want in order for Abra to properly ask 
the user for the information required to create transactions.  Once the user provides
the necessary information, an account is created at the exchange.  If the Account has
missing information more can be provided as well as documentation.

### Happy Path
1. **[Get Payment Methods](#get-payment-methods) -** When a user wants to add a new way
to add money or withdraw money, they will first need to select a country.  From there
they will be shown what payment methods are available.  This result from this call
will help to populate that list.
2. **[Get Locations](#get-locations)** (Optional) **-** Some payment methods, such as ATM, 
are done at physical locations.  This call allows Abra to show its customers where these 
place are located.
3. **[Get Required Fields](#get-required-fields) -** Once a location has been selected, 
the user must fill in their personal information.  This call allows Abra to determine 
what information is required by the exchange for KYC purposes and to initiate 
withdraws/deposits.    
4. **[Post Account](#post-account) -** Once the user has provided Abra with all their 
information, Abra will direct it to the exchange through the Post Account call.  The
exchange creates a user account in their system and also provides Abra with the API
key for the user and account ID.
5. **[Post KYC](#post-kyc)** (Optional) **-** If the exchange accepts KYC information in 
the previous call, then this step would be skipped, otherwise, the KYC information would 
be directed to the exchange during this step.  Another possible situation is that the 
exchange has tiered KYC, and they don't require KYC at the lowest tier.  This step is 
skipped in that instance as well.
6. **[Post KYC Document](#post-kyc-document)** (Optional) **-** Like the previous call,
this is optional as well.  This will only be required if the KYC tier the customer is 
aiming for requires Documents.  

### If issues...
- **[Patch Account](#patch-account) -** If an account enters a HOLD or PENDING state, this
call is used to modify the account.  Normally an account will enter one of these states
because the User information appears wrong or more information is required.
- **[Patch KYC](#patch-kyc) -** Same purpose as described above.
- **[Post KYC Document](#post-kyc-document) -** Same purpose as above, but used only
to post new Documents.  If replacing a document is required, it needs to be deleted first.
- **[Delete KYC Document](#delete-kyc-document) -** Used to delete documents that need
to be replaced.

## Withdraw
During a withdraw, a user wants to take money out of their Abra wallet.  This can be for
taking Fiat out, to make a purchase, or for a variety of other reasons.
  
The BTC address of the user at the exchange is first retrieved.  A transaction of type
Deposit BTC is done.  The order books are retrieved in order to send a Limit Order to
sell the BTC.  Once the Limit Order is fulfilled a transaction of type Withraw Fiat is
created to move the Fiat out of the exchange into the endpoint chosen by the user.

### Happy Path
1. **[Get BTC Address](#get-btc-address) -** Abra retrieves the exchanges's BTC Address 
that the belongs to the user.
2. **[Post Transaction](#post-transaction) -**  A **BTC Deposit** transaction 
is created that informs the exchange of the actual BTC being placed on the blockchain 
and directed to the BTC address received in the previous call.
3. **[Get Order Books](#get-order-books) -** Viewing the current books will help to 
create Limit Orders that will be fulfilled in a timely manner, which is crucial for
 Abra transactions.
4. **[Post Limit Order](#post-limit-order) -** A Limit Order is created to actually
sell the BTC currently in the user's bitcoin account at the exchange.
5. **[Post Transaction](#post-transaction)** (Optional) **-** If Limit Order's are not
available, Abra creates a **Sell BTC** transaction using a quote from the exchange
in order to convert the BTC into Fiat.
6. **[Post Transaction](#post-transaction) -** After the BTC has been converted to Fiat, 
Abra will create a **Withdraw Fiat** transaction in order to move the money out of the 
exchange to the user's desired endpoint.

### If issues...
- **[Patch Limit Order](#patch-limit-order) -** This will only be used by the admin.
Normally limit orders will not be patched.  They instead will be deleted and a new one
will be created.  However, in exeptional situations, a patch might be required.
- **[Delete Limit Order](#delete-limit-order) -**  If a limit order is not being filled,
chances are the amount is too high or too low.  In that situation, the limit order will be
canceled so that a new one can be created.
- **[Delete Transaction](#delete-transaction) -**  If a user decided they don't want to go
through with a transaction, and the transaction has not been completed, a cancellation
might be possible.  The cancellation would be done through this call.

## Deposit
A deposit takes place when a user wants to put money into their Abra wallet.  To do this,
a user would have to visit an endpoint that accepts deposits.  Depositing to a bank 
account is a way for the user to do this.
  
The user first selects an endpoint from the list.  Abra will then return instructions to
the user on how to actually deposit money so that it goes to their exchange account.  Once
the exchange account has funds, the books are retrieved to come up with a well priced
limit order to purchase BTC.  The limit order is created and the BTC are purchased.  Abra
then tells the exchange to deposit the BTC into the user's Abra wallet.  The Deposit is
now considered complete.

### Happy Path
1. **[Get Account](#get-account) -** Abra first retrieves the user's Exchange Fiat account 
to return instructions on how to actually do a deposit into that account.
2. **[Post Transaction](#post-transaction) -** When the exchange receives the deposit, 
they need to let Abra know that there is money in the account.  They do this by creating 
a transaction of type **Deposit Fiat**.
3. **[Get Order Books](#get-order-books) -** Abra now needs to move the fiat into BTC.  
To do this, Abra must retrieve the books to see what a good price would be for a Limit 
Order.
4. **[Post Limit Order](#post-limit-order) -** Abra now creates a Limit Order to buy BTC 
using the Fiat in the user's exchange account.  The limit order must be priced so that 
it gets filled in a timely manner.
5. **[Post Transaction](#post-transaction)** (Optional) **-** If the exchange does not 
use Limit Orders Abra would have to send a Transaction of type **Buy BTC** in order 
to purchase the bitcoin.
6. **[Post Transaction](#post-transaction) -** Once the BTC is in the user's BTC account, 
Abra can now ask the exchange to deposit the bitcoin into the user's Abra Wallet by 
passing in a BTC address that belongs to the user. 

### If issues..
- **[Patch Limit Order](#patch-limit-order) -** This will only be used by the admin. 
Normally limit orders will not be patched.  They instead will be deleted and a new one 
will be created.  However, in exeptional situations, a patch might be required.
- **[Delete Limit Order](#delete-limit-order) -** If a limit order is not being filled, 
chances are the amount is too high or too low.  In that situation, the limit order will be 
canceled so that a new one can be created.
- **[Delete Transaction](#delete-transaction) -**  If a user decided they don't want to go
through with a transaction, and the transaction has not been completed, a cancellation
might be possible.  The cancellation would be done through this call.

## Notifications
Notifications are not their own flow, but fit into each of the flows mentioned above.  For
most steps in the 3 actions above, Abra can't continue onto the next step until a
notification is received that informs the completion of the previous step.  For example, 
Step 5 and Step 6 in Account Creation are not required unless a notification informing
Abra of an Account requiring more KYC information is received.  Likewise, Step 6 in
Withdraw and Deposit are not possible until a notification is received that the Limit Order
has been fulfilled and funds are available.

1. **[Get Notifications](#get-notifications) -** Abra retrieves notifications from 
exchange to update Orders, Transactions and Accounts.
2. **[Delete Notification](#delete-notification) -** Once a notification has been processed,
Abra deletes it so that they don't receive it again.