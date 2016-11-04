---
title: Azure Data Lake Analytics'i Node.js için Azure SDK'yı kullanarak yönetme | Microsoft Docs
description: Data Lake Analytics hesaplarını, veri kaynaklarını, işlerini ve kullanıcılarını Node.js için Azure SDK'yı kullanarak yönetmeyi öğrenin
services: data-lake-analytics
documentationcenter: ''
author: edmacauley
manager: jhubbard
editor: cgronlun

ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/16/2016
ms.author: edmaca

---
# Azure Data Lake Analytics'i Node.js için Azure SDK'yı kullanarak yönetme
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

Node.js için Azure SDK, Azure Data Lake Analytics hesaplarını, işlerini ve kataloglarını yönetmek için kullanılabilir. Diğer araçları kullanarak yönetme konu başlığını görmek için yukarıdaki sekme seçimine tıklayın.

Şu anda aşağıdakiler desteklenmektedir:

* **Node.js sürümü: 0.10.0 veya üzeri**
* **Hesap için REST API sürümü: 2015-10-01-önizleme**
* **Katalog için REST API sürümü: 2015-10-01-önizleme**
* **İş için REST API sürümü: 2016-03-20-önizleme**

## Özellikler
* Hesap yönetimi: oluşturma, alma, listeleme, güncelleştirme ve silme.
* İş yönetimi: gönderme, alma, listeleme, iptal etme.
* Katalog yönetimi: alma, listeleme, oluşturma (gizli anahtarlar), güncelleştirme (gizli anahtarlar), silme (gizli anahtarlar).

## Yükleme
```bash
npm install azure-arm-datalake-analytics
```

## Azure Active Directory'yi kullanarak kimlik doğrulama
 ```javascript
 var msrestAzure = require('ms-rest-azure');
 //user authentication
 var credentials = new msRestAzure.UserTokenCredentials('your-client-id', 'your-domain', 'your-username', 'your-password', 'your-redirect-uri');
 //service principal authentication
 var credentials = new msRestAzure.ApplicationTokenCredentials('your-client-id', 'your-domain', 'your-secret');
 ```

## Data Lake Analytics istemcisi oluşturma
```javascript
var adlaManagement = require("azure-arm-datalake-analytics");
var acccountClient = new adlaManagement.DataLakeAnalyticsAccountClient(credentials, 'your-subscription-id');
var jobClient = new adlaManagement.DataLakeAnalyticsJobClient(credentials, 'azuredatalakeanalytics.net');
var catalogClient = new adlaManagement.DataLakeAnalyticsCatalogClient(credentials, 'azuredatalakeanalytics.net');
```

## Data Lake Analytics hesabı oluşturma
```javascript
var util = require('util');
var resourceGroupName = 'testrg';
var accountName = 'testadlaacct';
var location = 'eastus2';

// A Data Lake Store account must already have been created to create
// a Data Lake Analytics account. See the Data Lake Store readme for
// information on doing so. For now, we assume one exists already.
var datalakeStoreAccountName = 'existingadlsaccount';

// account object to create
var accountToCreate = {
  tags: {
    testtag1: 'testvalue1',
    testtag2: 'testvalue2'
  },
  name: accountName,
  location: location,
  properties: {
    defaultDataLakeStoreAccount: datalakeStoreAccountName,
    dataLakeStoreAccounts: [
      {
        name: datalakeStoreAccountName
      }
    ]
  }
};

client.account.create(resourceGroupName, accountName, accountToCreate, function (err, result, request, response) {
  if (err) {
    console.log(err);
    /*err has reference to the actual request and response, so you can see what was sent and received on the wire.
      The structure of err looks like this:
      err: {
        code: 'Error Code',
        message: 'Error Message',
        body: 'The response body if any',
        request: reference to a stripped version of http request
        response: reference to a stripped version of the response
      }
    */
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## İşlerin listesini alma
```javascript
var util = require('util');
var accountName = 'testadlaacct';
jobClient.job.list(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Data Lake Analytics Kataloğu'ndaki veritabanlarının listesini alma
```javascript
var util = require('util');
var accountName = 'testadlaacct';
catalogClient.catalog.listDatabases(accountName, function (err, result, request, response) {
  if (err) {
    console.log(err);
  } else {
    console.log('result is: ' + util.inspect(result, {depth: null}));
  }
});
```

## Ayrıca bkz.
* [Node.js için Microsoft Azure SDK](https://github.com/azure/azure-sdk-for-node)
* [Node.js için Microsoft Azure SDK - Data Lake Store Yönetimi](https://github.com/Azure/azure-sdk-for-node/tree/autorest/lib/services/dataLake.Store)

<!--HONumber=Sep16_HO3-->


