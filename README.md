---
description: DLT based syndicated loan platform
---

# About SyndLend

**SyndLend is a Distributed ledger technology based syndicated loan platform to create a speedy, cost efficient, transparent yet compliant market place between loan borrowers, investors and arranging banks**

## Features

{% tabs %}
{% tab title="Convenient and efficient" %}
Instant and easy deployment to devise an efficient syndicated loan market
{% endtab %}

{% tab title="Transparent and privacy preserved" %}
Each entity receives all the relevant information about the loan, market status without having to trust the single source
{% endtab %}

{% tab title="Quicker settlement" %}
Integration of the token system and instant verification through smart contracts speeds up the settlement time
{% endtab %}
{% endtabs %}

## Architecture

SyndLend is architected in such a way that the participants, namely, loan borrowers, lenders, agents, and arranging banks share and communicate deal information on a need to know basis by creating private transactions.

![](.gitbook/assets/3731872a6cddad86f3fd37ea2badf080.png)

* 
## Implementation Phases

{% tabs %}
{% tab title="Loan trade desk - Phase 1" %}
A marketplace where loan requests and proposals are validated and finally loans are syndicated

* Loan **borrower** creates a loan request by filling the application
* **Arranger** reviews the loan request and does background check on/ off ledger with the help of verification bodies
* Arranger broadcasts the loan information to the trade desk so that lenders can review the applications
* **Lenders** proposes for lending with few details
* Arranger either accepts/ rejects or asks to amend the proposal
* Finally arranger can syndicate the loan and send the details to the borrower
{% endtab %}

{% tab title="Loan disbursement - Phase 2" %}
An obligation will be created to extend the loan to the borrower after the loan was finalized and syndicated.

* **Lender** initiates the disbursal process
* Borrower selects the type of payment/ settlement 
* Lender approves the payment
{% endtab %}

{% tab title="Loan repayment & defaults - Phase 3" %}
Regular loan repayments as per schedule and loan default scenarios are handled here.

* Borrower pays the installments whenever it is due
* In case of failure to stick to the contract, borrower will notified about the future events 
* Default scenarios will be triggered after certain thresholds
{% endtab %}
{% endtabs %}

