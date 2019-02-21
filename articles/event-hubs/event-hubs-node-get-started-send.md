---
title: Node.js - Azure Event Hubs kullanarak olayları gönderme | Microsoft Docs
description: Bu makalede, Azure Event Hubs'dan olayları gönderen bir Node.js uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 02/19/2019
ms.author: spelluru
ms.openlocfilehash: ec3182d11f1b2ffa31acd05fa1f2db695f3f2cf7
ms.sourcegitcommit: 6cab3c44aaccbcc86ed5a2011761fa52aa5ee5fa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/20/2019
ms.locfileid: "56447727"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Node.js kullanarak Azure Event Hubs için olayları gönderme

Azure Event Hubs, bir büyük veri platformu ve alabilir, olay alma hizmetidir ve saniyede milyonlarca işlem akışı. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide, node.js'de yazılmış bir uygulamadan bir olay hub'ına olayları göndermek nasıl açıklar.

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Node.js sürümü 8.x ve daha yüksek. En son LTS sürümü [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (önerilir) veya diğer herhangi bir IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md), sonra Bu öğreticide aşağıdaki adımlarla devam edin.

Bağlantı dizesi olay hub'ı ad alanı için makaledeki yönergeleri izleyerek alın: [Bağlantı dizesini alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticide daha sonra'de bağlantı dizesini kullanın.

## <a name="clone-the-sample-git-repository"></a>Örnek Git deposunu kopyalayın
Örnek Git deposundan kopyalama [GitHub](https://github.com/Azure/azure-event-hubs-node) makinenizde. 

## <a name="install-nodejs-package"></a>Node.js paketini yükle
Node.js paketi için Azure Event Hubs makinenize yükleyin. 

```shell
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Git deposunu kopyalayın
İndirin veya kopyalayın [örnek](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) github'dan. 

## <a name="send-events"></a>Olayları gönderme
Kopyalanmış SDK, node.js kullanarak bir olay hub'ına olayları göndermek nasıl gösteren birden çok örnekler içerir. Bu hızlı başlangıçta, kullandığınız **simpleSender.js** örnek. Alınan olayları gözlemektir için başka bir terminal açın ve kullanarak olay alma [örnek alma](event-hubs-node-get-started-receive.md).

1. Visual Studio Code projede açın. 
2. Adlı bir dosya oluşturun **.env** altında **istemci** klasör. Örnek ortam değişkenlerinden kopyalayıp **sample.env** kök klasöründe.
3. Olay hub'ı bağlantı dizesi, olay hub'ı adı ve depolama uç noktası yapılandırın. Bir event hub için bir bağlantı dizesi alma yönergeleri [bağlantı dizesi alma](event-hubs-create.md#create-an-event-hubs-namespace).
4. Azure CLI gidin **istemci** klasör yolu. Düğüm paketleri yükleyin ve aşağıdaki komutları çalıştırarak projeyi derleyin:

    ```shell
    npm i
    npm run build
    ```
5. Aşağıdaki komutu çalıştırarak olayları göndermeye başlayın: 

    ```shell
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin 
Örnek kodu simpleSender.js dosyasında bir olay hub'ına olayları göndermek için gözden geçirin.

```javascript
"use strict";
Object.defineProperty(exports, "__esModule", { value: true });
const lib_1 = require("../lib");
const dotenv = require("dotenv");
dotenv.config();
const connectionString = "EVENTHUB_CONNECTION_STRING";
const entityPath = "EVENTHUB_NAME";
const str = process.env[connectionString] || "";
const path = process.env[entityPath] || "";

async function main() {
    const client = lib_1.EventHubClient.createFromConnectionString(str, path);
    const data = {
        body: "Hello World!!"
    };
    const delivery = await client.send(data);
    console.log(">>> Sent the message successfully: ", delivery.tag.toString());
    console.log(delivery);
    console.log("Calling rhea-promise sender close directly. This should result in sender getting reconnected.");
    await Object.values(client._context.senders)[0]._sender.close();
    // await client.close();
}

main().catch((err) => {
    console.log("error: ", err);
});

```

Betiği çalıştırmadan önce ortam değişkenlerini ayarlamak unutmayın. Komut satırında aşağıdaki örnekte gösterilen şekilde yapılandırın veya kullanabilirsiniz [dotenv paket](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, Node.js kullanarak bir olay hub'ına ileti gönderdiniz. Node.js kullanarak bir olay hub'ından olay alma konusunda bilgi almak için bkz: [event hub'dan - Node.js olayları alma](event-hubs-node-get-started-receive.md)

Event Hubs için diğer Node.js örneklerini gözden kontrol [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
