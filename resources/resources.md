---
description: Collection of the tools developed for SyndLend
---

# SyndLend resources

## SyndLend Core

Core software of SyndLend which can be deployed by any entity involved in the loan syndication marketplace.  It is hosted in a private repository. Contact us at [info@consensolabs.com](mailto:info@consensolabs.com?subject=SyndLend%20trial) for demo application.

{% hint style="info" %}
This software is a collection of all the CorDapps needed to accommodate all the phases of loan syndication.
{% endhint %}

## [SyndLend Client](https://github.com/consensolabs/syndlend-client)

A front-end React application to communicate with the Corda node. It provides the entire end to end flow by communicating with all the flows of CorDapps exposed through RPC interface. The source code is hosted in a public repository.

```text
git clone https://github.com/consensolabs/syndlend-client
```

## SyndLend KYC

The entities which perform due diligence need to carry out KYC operations. We have created a centralized KYC application that can be used by these due diligence nodes. The source code is hosted in a public repository.

{% tabs %}
{% tab title="KYC Client" %}
```text
git clone https://github.com/consensolabs/syndlend-kyc-client
```
{% endtab %}

{% tab title="KYC Server" %}
```text
git clone https://github.com/consensolabs/syndlend-kyc
```
{% endtab %}
{% endtabs %}

## SyndLend deployment

All the deployment strategies for the entities of SyndLend are maintained here in a public repository.

```text
git clone https://github.com/consensolabs/syndlend-devops
```

## SyndLend loan syndication CorDapp

The loan syndication CorDapp is responsible for syndicating the loan by providing a trade desk for loan requests and loan proposals. It is hosted in a private repository. Contact us at [info@consensolabs.com](mailto:info@consensolabs.com?subject=SyndLend%20trial) for demo application.



