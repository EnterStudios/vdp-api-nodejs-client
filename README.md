# vdp-api-nodejs-client

> Disclaimer: Disclaimer: This is a reference implementation of an Experimental Tools and SDKs described at [Visa Developer Center – Innovation Corner](https://developer.visa.com/innovation-corner). This is not intended for production use.

A Node.js client library for using Visa APIs. It includes support for authorization and authentication with Mutual Auth and X Pay Token. To use this library, you need to setup your owner server. This library is designed to simplify calling Visa APIs so that you can focus on building the frontend of your application.

## Table of Contents

* [Get Credentials](#get-credentials)
* [Installation](#installation)
* [Supported APIs](#supported-apis)
* [Usage](#usage)
  * [Client Initialisation](#client-initialisation)
  * [mVisa](#mvisa)
  * [fundstransfer](#fundstransfer)
  * [Run the tests](#run-the-tests)
  * [Best practices](#best-practices)
  * [License](#license)


## Get Credentials
Obtain your credentials from the Visa Developer Platform. [Click here](https://developer.visa.com/vdpguide#get-started-overview) to get started.

## Installation

This library is distributed on npm. In order to add it as a dependency, run the following command:

`npm install git+https://github.com/visa/vdp-api-nodejs-client.git --save`

### Dependency
This library uses the [vdp-nodejs-client](https://github.com/visa-innovation-sf/vdp-nodejs-client) library to call VDP Gateway which will be installed automatically when you use the above command. 

## Supported APIs

- mVisa : [VDP Documentation](https://developer.visa.com/capabilities/visa_direct/reference#visa_direct__mvisa) , [SDK Documentation](#mvisa)

- fundstransfer : [VDP Documentation](https://developer.visa.com/capabilities/visa_direct/reference#visa_direct__funds_transfer) , [SDK Documentation](#fundstransfer)

## Usage

### Client Initialisation
To interact with the Visa APIs you need to create a Visa API Client using your credentials. You need to pass at least one type of credentials to this client based on the APIs you intend to use. 

```javascript
const VisaAPIClient = require('vdp-api-nodejs-client');

const instance = new VisaAPIClient({
    mutualAuthCredentials: {
      userId: 'XXX'
      password: 'XXX'
      keyFile: '/Users/abc/vdp/myPrivateKey.pem'
      certificateFile: '/Users/abc/vdp/cert.pem'
    },
    XPayTokenCredentials: {
      sharedSecret: 'XXX'
    },
    visaUrl: 'https://sandbox.api.visa.com/'
});
```

### mVisa

API - *MerchantPushPayments* POST

Client Method - *payMerchant*

Makes a push payment to a merchant with a Visa PAN. Merchant data can be read using a QR Code.

Name  | Type | Description
------------- | ------------- | ---------------- |
merchant  | Object |  merchant object |
sender  | Object | sender object |
transaction | Object | transaction object |


```javascript

let merchant = {
    acquiringBin: '451478',
    city: 'BIG TOWN',
    countryCode: '356',
    pan: 'xxxxxxxxxxx'
    name: 'MARY JANE'
}

let sender = {
    accountNumber: 'xxxxxxxxxxxx'
    name: 'JOHN DOE'
}

let transaction = {
    amount: '100',
    currencyCode: 'INR'
}

instance.mvisa.payMerchant(merchant, sender, transaction)
  .then(response => {
    //do something with the response
  })
  .catch(err => {
    //do something with the err
  })
```

### fundstransfer

API - *PushPayments* POST

Client Method - *pushFunds*

Makes a push payment to a receiver with a Visa PAN.

Name  | Type | Description
------------- | ------------- | ---------------- |
receiver  | Object |  merchant object |
sender  | Object | sender object |
transaction | Object | transaction object |


```javascript

let receiver = {
    acquiringBin: '451478',
    countryCode: '840',
    pan: 'xxxxxxxx',
    name: 'MARY JANE',
    state: 'CA',
    zipCode: '94105'
}

let sender = {
    accountNumber: 'xxxxxxxxx'
    name: 'JOHN DOE'
}

let transaction = {
    amount: '100',
    currencyCode: 'USD'
}

instance.fundstransfer.pushFunds(receiver, sender, transaction)
  .then(response => {
    //do something with the response
  })
  .catch(err => {
    //do something with the err
  })
```

### Run the tests

Running all the tests:

```
npm test
```

### Best Practices

The client code provided reads the credentials as plain text. As a best practice we recommend you to store the credentials in an encrypted format and decrypt while using them.

### License

© Copyright 2018 Visa. All Rights Reserved.

NOTICE: The software and accompanying information and documentation (together, the “Software”) remain the property of and are proprietary to Visa and its suppliers and affiliates. The Software remains protected by intellectual property rights and may be covered by U.S. and foreign patents or patent applications. The Software is licensed and not sold.

By accessing the Software you are agreeing to Visa's terms of use (developer.visa.com/terms) and privacy policy (developer.visa.com/privacy). In addition, all permissible uses of the Software must be in support of Visa products, programs and services provided through the Visa Developer Program (VDP) platform only (developer.visa.com). THE SOFTWARE AND ANY ASSOCIATED INFORMATION OR DOCUMENTATION IS PROVIDED ON AN “AS IS,” “AS AVAILABLE,” “WITH ALL FAULTS” BASIS WITHOUT WARRANTY OR CONDITION OF ANY KIND. YOUR USE IS AT YOUR OWN RISK.

All brand names are the property of their respective owners, used for identification purposes only, and do not imply product endorsement or affiliation with Visa. Any links to third party sites are for your information only and equally do not constitute a Visa endorsement. Visa has no insight into and control over third party content and code and disclaims all liability for any such components, including continued availability and functionality. Benefits depend on implementation details and business factors and coding steps shown are exemplary only and do not reflect all necessary elements for the described capabilities. Capabilities and features are subject to Visa’s terms and conditions and may require development, implementation and resources by you based on your business and operational details. Please refer to the specific API documentation for details on the requirements, eligibility and geographic availability.

This Software includes programs, concepts and details under continuing development by Visa. Any Visa features, functionality, implementation, branding, and schedules may be amended, updated or canceled at Visa’s discretion. The timing of widespread availability of programs and functionality is also subject to a number of factors outside Visa’s control, including but not limited to deployment of necessary infrastructure by issuers, acquirers, merchants and mobile device manufacturers.
