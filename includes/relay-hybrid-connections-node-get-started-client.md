---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: 9d4f7faa18ee7fae158afb42b8c42287e61dd103
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66150342"
---
### <a name="create-a-nodejs-application"></a>Node.js uygulaması oluşturma

`sender.js` adlı yeni bir JavaScript dosyası oluşturun.

### <a name="add-the-relay-npm-package"></a>Geçiş NPM paketini ekleme

Proje klasörünüzdeki bir Düğüm komut isteminden `npm install hyco-ws` komutunu çalıştırın.

### <a name="write-some-code-to-send-messages"></a>İleti göndermek için bazı kodlar yazma

1. Aşağıdaki `constants` öğesini `sender.js` dosyasının başına ekleyin.
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
    ```
2. Karma bağlantı ayrıntıları için şu sabitleri `sender.js` dosyasına ekleyin. Köşeli ayraçlar içindeki yer tutucuları, karma bağlantıyı oluştururken aldığınız değerlerle değiştirin.
   
   1. `const ns` - Geçiş ad alanı. Tam ad alanı adını kullandığınızdan emin olun: örneğin, `{namespace}.servicebus.windows.net`.
   2. `const path` - Karma bağlantının adı.
   3. `const keyrule` - SAS anahtarının adı.
   4. `const key` - SAS anahtarının değeri.

3. `sender.js` dosyasına aşağıdaki kodu ekleyin:
   
    ```js
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```
    Sender.js dosyanız aşağıdaki gibi görünmelidir:
   
    ```js
    const WebSocket = require('hyco-ws');
    const readline = require('readline')
        .createInterface({
            input: process.stdin,
            output: process.stdout
        });;
   
    const ns = "{RelayNamespace}";
    const path = "{HybridConnectionName}";
    const keyrule = "{SASKeyName}";
    const key = "{SASKeyValue}";
   
    WebSocket.relayedConnect(
        WebSocket.createRelaySendUri(ns, path),
        WebSocket.createRelayToken('http://'+ns, keyrule, key),
        function (wss) {
            readline.on('line', (input) => {
                wss.send(input, null);
            });
   
            console.log('Started client interval.');
            wss.on('close', function () {
                console.log('stopping client interval');
                process.exit();
            });
        }
    );
    ```

