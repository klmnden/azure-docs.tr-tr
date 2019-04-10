---
title: Azure Media Services v3 API'sine - Node.js bağlanma
description: Media Services v3 API'si Node.js ile bağlanma hakkında bilgi edinin.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: juliako
ms.openlocfilehash: 7f3afa59b4c8eaaeaf54576eb9fcaad626749683
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59358907"
---
# <a name="connect-to-media-services-v3-api---nodejs"></a>Media Services v3 API'sine - Node.js bağlanma

Bu makalede, Azure Media Services v3 node.js SDK'sı hizmet sorumlusu oturum açma yöntemini kullanarak bağlanma gösterir.

## <a name="prerequisites"></a>Önkoşullar

- Yükleme [Node.js](https://nodejs.org/en/download/).
- [Bir Media Services hesabı oluşturma](create-account-cli-how-to.md). Kaynak grubu adı ve Media Services hesap adını hatırlamak emin olun.

## <a name="create-packagejson"></a>Package.JSON oluşturma

1. Tercih ettiğiniz düzenleyiciyi kullanarak bir package.json dosyası oluşturun.
1. Dosyasını açın ve aşağıdaki kodu yapıştırın:

```json
{
  "name": "media-services-node-sample",
  "version": "0.1.0",
  "description": "",
  "main": "./index.js",
  "dependencies": {
    "azure-arm-mediaservices": "^4.1.0",
    "azure-storage": "^2.8.0",
    "ms-rest": "^2.3.3",
    "ms-rest-azure": "^2.5.5"
  }
}
```

Aşağıdaki paketler belirtilmelidir:

|Paket|Açıklama|
|---|---|
|`azure-arm-mediaservices`|Azure Media Services SDK. <br/>En son Azure Media Services paketini kullandığınızdan emin olmak için kontrol [NPM yükleme azure arm mediaservices](https://www.npmjs.com/package/azure-arm-mediaservices/).|
|`azure-storage`|Depolama SDK'sı. Karşıya dosya yükleme varlıklarına olduğunda kullanılır.|
|`ms-rest-azure`| Oturum açmak için kullanılır.|

En son paketini kullandığınızdan emin olmak için aşağıdaki komutu çalıştırabilirsiniz:

```
npm install azure-arm-mediaservices
```

## <a name="connect-to-nodejs-client"></a>Node.js istemcisi bağlanma

1. Tercih ettiğiniz düzenleyiciyi kullanarak bir .js dosyası oluşturun.
1. Dosyasını açın ve aşağıdaki kodu yapıştırın.
1. Değerler "uç nokta yapılandırma" bölümündeki, aradığınızı değerlerini ayarlayın [API'lere](access-api-cli-how-to.md).

```js
'use strict';

const MediaServices = require('azure-arm-mediaservices');
const msRestAzure = require('ms-rest-azure');
const msRest = require('ms-rest');
const azureStorage = require('azure-storage');

// endpoint config
// make sure your URL values end with '/'
const armAadAudience = "";
const aadEndpoint = "";
const armEndpoint = "";
const subscriptionId = "";
const accountName = "";
const region = "";
const aadClientId = "";
const aadSecret = "";
const aadTenantId = "";
const resourceGroup = "";

let azureMediaServicesClient;

///////////////////////////////////////////
//     Entrypoint for sample script      //
///////////////////////////////////////////

msRestAzure.loginWithServicePrincipalSecret(aadClientId, aadSecret, aadTenantId, {
  environment: {
    activeDirectoryResourceId: armAadAudience,
    resourceManagerEndpointUrl: armEndpoint,
    activeDirectoryEndpointUrl: aadEndpoint
  }
}, async function(err, credentials, subscriptions) {
    if (err) return console.log(err);
    azureMediaServicesClient = new MediaServices(credentials, subscriptionId, armEndpoint, { noRetryPolicy: true });
    
    console.log("connected");

});
```

## <a name="run-your-app"></a>Uygulamanızı çalıştırma

Bir komut istemi açın. Örnek kullanıcının dizinine gidin ve aşağıdaki komutları yürütün:

```
npm install 
node index.js
```

## <a name="see-also"></a>Ayrıca bkz.

[.NET başvurusu](https://docs.microsoft.com/dotnet/api/overview/azure/mediaservices/management?view=azure-dotnet)

## <a name="next-steps"></a>Sonraki adımlar

- [Media Services kavramları](concepts-overview.md)
- [Node.js başvurusu](https://docs.microsoft.com/javascript/api/azure-arm-mediaservices/?view=azure-node-latest)
- [NPM yükleme azure-arm-mediaservices](https://www.npmjs.com/package/azure-arm-mediaservices/)<br>
