---
description: >-
  List of APIs exposed by the SyndLend nodes that can be consumed by the
  front-end applications.
---

# SyndLend APIs

## Test HTTP interface to communicate with the RPC methods \(Corda Braid service\)

Endpoints are only for testing. Make RPC calls directly to the node in the client applications.

### Quick start

Make sure you have the latest version of node and npm installed

```text
git clone https://github.com/consensolabs/syndlend-client
cd tests
npm install
node client.js
```

#### Get all the nodes in the network \[GET\]:

```text
/service/participants
```

#### Get all the loan requests \[GET\]:

```text
/service/loan/requests
```

#### Get details of a loan request \[GET\]:

Replace `loanid` with the actual loan ID

```text
/service/loan/:loanid/request
```

**Invoker: Borrower Party**

#### Create a new loan request \[POST\]:

```text
/service/loan/request
```

**Invoker: Verifier Party \(Manual trigger for now\)**

#### Verify a loan request \[POST\]:

```text
/service/loan/verify
```

**Invoker: Agent Party**

#### Issue a new loan loan \[POST\]:

```text
/service/loan/issue
```

**Invoker: Agent/ Lender Party**

#### List all the issue loans \[GET\]:

```text
/service/loan/issued
```

**Invoker: Lender Party**

#### Propose a lend request \[POST\]:

```text
service/propose/request
```

**Invoker: Agent/ Borrower Party**

#### List proposal \[GET\]:

```text
/service/propose/request
```

**Invoker: Agent Party**

#### Propose a lend response\[POST\]:

```text
service/propose/response
```

**Invoker: Agent Party**

#### Syndicate the loan \[POST\]:

```text
/service/loan/syndicate
```

