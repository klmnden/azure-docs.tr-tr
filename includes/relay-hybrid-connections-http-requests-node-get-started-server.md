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
ms.openlocfilehash: fdd4e26839661ab9765b7d496e7f60c3686ba637
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60553949"
---
### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

`listener.js` adlı yeni bir JavaScript dosyası oluşturun.

### <a name="add-the-relay-npm-package"></a>Geçiş NPM paketini ekleme

Proje klasörünüzdeki bir Düğüm komut isteminden `npm install hyco-https` komutunu çalıştırın.

### <a name="write-some-code-to-handle-requests"></a>İstekleri işlemeye yönelik kod yazın

1. Aşağıdaki sabiti `listener.js` dosyasının başına ekleyin.

    ```js
    const https = require('hyco-https');
    ```
2. Karma bağlantı ayrıntıları için şu sabitleri `listener.js` dosyasına ekleyin. Köşeli ayraçlar içindeki yer tutucuları, karma bağlantıyı oluştururken aldığınız değerlerle değiştirin.

   1. `const ns` - Geçiş ad alanı. Tam ad alanı adını kullandığınızdan emin olun: örneğin, `{namespace}.servicebus.windows.net`.
   2. `const path` - Karma bağlantının adı.
   3. `const keyrule` - SAS anahtarının adı.
   4. `const key` - SAS anahtarının değeri.

3. `listener.js` dosyasına aşağıdaki kodu ekleyin. :

    Bu kodun, normalde kullanılan `createServer` işlevi yerine `createRelayedServer` kullanılması dışında yeni başlayanlara yönelik Node.js öğreticilerinde bulabileceğiniz herhangi bir basit HTTP sunucusu kodundan çok da farklı olmadığını görebilirsiniz.

    ```js
    var uri = https.createRelayListenUri(ns, path);
    var server = https.createRelayedServer(
        {
            server : uri,
            token : () => https.createRelayToken(uri, keyrule, key)
        },
        (req, res) => {
            console.log('request accepted: ' + req.method + ' on ' + req.url);
            res.setHeader('Content-Type', 'text/html');
            res.end('<html><head><title>Hey!</title></head><body>Relayed Node.js Server!</body></html>');
        });

    server.listen( (err) => {
            if (err) {
              return console.log('something bad happened', err)
            }
            console.log(`server is listening on ${port}`)
          });

    server.on('error', (err) => {
        console.log('error: ' + err);
    });
    ```
    Listener.js dosyanız şu şekilde görünmelidir:
   
    ```js
    const https = require('hyco-https');
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    var uri = https.createRelayListenUri(ns, path);
    var server = https.createRelayedServer(
        {
            server : uri,
            token : () => https.createRelayToken(uri, keyrule, key)
        },
        (req, res) => {
            console.log('request accepted: ' + req.method + ' on ' + req.url);
            res.setHeader('Content-Type', 'text/html');
            res.end('<html><head><title>Hey!</title></head><body>Relayed Node.js Server!</body></html>');
        });

    server.listen( (err) => {
            if (err) {
              return console.log('something bad happened', err)
            }
            console.log(`server is listening on ${port}`)
          });

    server.on('error', (err) => {
        console.log('error: ' + err);
    });
    ```

