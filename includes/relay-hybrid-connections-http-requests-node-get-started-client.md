---
title: include dosyası
description: include dosyası
services: service-bus-relay
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 05/02/2018
ms.author: clemensv
ms.custom: include file
ms.openlocfilehash: 3c18efa7eb520b765c9bb3c2aff00104f971f5a8
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553971"
---
### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

Relay’i oluştururken "İstemci Kimlik Doğrulaması Gerektirir" seçeneğini devre dışı bıraktıysanız dilediğiniz tarayıcıyla Karma Bağlantılar URL’sine istek gönderebilirsiniz. Korumalı uç noktalara erişmek için burada gösterilen `ServiceBusAuthorization` üst bilgisinde bir belirteç oluşturup geçirmeniz gerekir.

Başlamak için `sender.js` adlı yeni bir JavaScript dosyası oluşturun.

### <a name="add-the-relay-npm-package"></a>Geçiş NPM paketini ekleme

Proje klasörünüzdeki bir Düğüm komut isteminden `npm install hyco-https` komutunu çalıştırın. Bu paket normal `https` paketini de içeri aktarır. İstemci tarafı için en temel fark, paketin Relay URI’leri ve belirteçleri oluşturmaya yönelik işlevler sağlamasıdır.

### <a name="write-some-code-to-send-messages"></a>İleti göndermek için bazı kodlar yazma

1. Aşağıdaki `constants` öğesini `sender.js` dosyasının başına ekleyin.
   
    ```js
    const https = require('hyco-https');
    ```

2. Karma bağlantı ayrıntıları için şu sabitleri `sender.js` dosyasına ekleyin. Köşeli ayraçlar içindeki yer tutucuları, karma bağlantıyı oluştururken aldığınız değerlerle değiştirin.
   
   1. `const ns` - Geçiş ad alanı. Tam ad alanı adını kullandığınızdan emin olun: örneğin, `{namespace}.servicebus.windows.net`.
   2. `const path` - Karma bağlantının adı.
   3. `const keyrule` - SAS anahtarının adı.
   4. `const key` - SAS anahtarının değeri.

3. `sender.js` dosyasına aşağıdaki kodu ekleyin. Kodun normal Node.js HTTPS istemcisini kullanmaktan pek farklı olmadığını, yalnızca yetkilendirme üst bilgisini eklediğini fark edebilirsiniz.
   
    ```js
   https.get({
        hostname : ns,
        path : (!path || path.length == 0 || path[0] !== '/'?'/':'') + path,
        port : 443,
        headers : {
            'ServiceBusAuthorization' : 
                https.createRelayToken(https.createRelayHttpsUri(ns, path), keyrule, key)
        }
    }, (res) => {
        let error;
        if (res.statusCode !== 200) {
            console.error('Request Failed.\n Status Code: ${statusCode}');
            res.resume();
        } 
        else {
            res.setEncoding('utf8');
            res.on('data', (chunk) => {
                console.log(`BODY: ${chunk}`);
            });
            res.on('end', () => {
                console.log('No more data in response.');
            });
        };
    }).on('error', (e) => {
        console.error(`Got error: ${e.message}`);
    });
    ```
    Sender.js dosyanız aşağıdaki gibi görünmelidir:
   
    ```js
    const https = require('hyco-https');
       
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    https.get({
        hostname : ns,
        path : (!path || path.length == 0 || path[0] !== '/'?'/':'') + path,
        port : 443,
        headers : {
            'ServiceBusAuthorization' : 
                https.createRelayToken(https.createRelayHttpsUri(ns, path), keyrule, key)
        }
    }, (res) => {
        let error;
        if (res.statusCode !== 200) {
            console.error('Request Failed.\n Status Code: ${statusCode}');
            res.resume();
        } 
        else {
            res.setEncoding('utf8');
            res.on('data', (chunk) => {
                console.log(`BODY: ${chunk}`);
            });
            res.on('end', () => {
                console.log('No more data in response.');
            });
        };
    }).on('error', (e) => {
        console.error(`Got error: ${e.message}`);
    });
    ```

