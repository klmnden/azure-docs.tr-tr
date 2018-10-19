---
title: Node.js kullanarak Azure Event Hubs için olayları gönderme | Microsoft Docs
description: Node.js kullanarak Event Hubs'a olay göndermeye başlayın.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 09/18/2018
ms.author: shvija
ms.openlocfilehash: bb5a7b477b2d19c74cc645a15cc3d891c76f28c5
ms.sourcegitcommit: 707bb4016e365723bc4ce59f32f3713edd387b39
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/19/2018
ms.locfileid: "49427204"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Node.js kullanarak Azure Event Hubs için olayları gönderme

Azure Event Hubs, milyonlarca işleyebilen ileri düzeyde ölçeklenebilir bir olay yönetim sistemi saniyede olayları, işlemek ve çok büyük miktardaki verileri çözümlemek uygulamaları etkinleştirme bağlı cihazlarınız ve diğer sistemleri tarafından üretilen. Bir olay hub'ına toplandıktan sonra almak ve işlem içi işleyicileri kullanarak olayları işleme veya diğer analiz sistemleri ileterek.

Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış](event-hubs-about.md).

Bu öğreticide, node.js'de yazılmış bir uygulamadan bir olay hub'ına olayları göndermek nasıl açıklar. Node.js olay işleyicisi ana bilgisayarı paketi kullanarak olayları almak için bkz. [karşılık gelen alma makale](event-hubs-node-get-started-receive.md).

Bu hızlı başlangıç kullanılabilir kodu [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client). 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Node.js sürümü 8.x ve daha yüksek. En son LTS sürümü [ https://nodejs.org ](https://nodejs.org).
- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.
- Visual Studio Code (önerilir) veya diğer herhangi bir IDE

## <a name="create-a-namespace-and-event-hub"></a>Ad alanı ve olay hub'ı oluşturma
İlk adım, bir olay hub'ı ile bir Event Hubs ad alanı oluşturmak için Azure portalı kullanmaktır. Mevcut bir yoksa, yönergeleri izleyerek bu varlıkları oluşturabilirsiniz [bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma](event-hubs-create.md).

## <a name="clone-the-sample-git-repository"></a>Örnek Git deposunu kopyalayın
Örnek Git deposundan kopyalama [Github](https://github.com/Azure/azure-event-hubs-node) makinenizde. 

## <a name="install-nodejs-package"></a>Node.js paketini yükle
Node.js paketi için Azure Event Hubs makinenize yükleyin. 

```nodejs
npm install @azure/event-hubs
```

## <a name="clone-the-git-repository"></a>Git deposunu kopyalayın
İndirin veya kopyalayın [örnek](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) github'dan. 

## <a name="send-events"></a>Olayları gönderme
Kopyalanmış SDK, node.js kullanarak bir olay hub'ına olayları göndermek nasıl gösteren birden çok örnekler içerir. Bu hızlı başlangıçta, kullandığınız **simpleSender.js** örnek. Alınan olayları gözlemektir için başka bir terminal açın ve kullanarak olay alma [örnek alma](event-hubs-node-get-started-receive.md).

1. Visual Studio Code projede açın. 
2. Adlı bir dosya oluşturun **.env** altında **istemci** klasör. Örnek ortam değişkenlerinden kopyalayıp **sample.env** kök klasöründe.
3. Olay hub'ı bağlantı dizesi, olay hub'ı adı ve depolama uç noktası yapılandırın. Olay hub'ından için bağlantı dizesini kopyalayabilirsiniz **bağlantı dizesi-birincil** anahtar altında **RootManageSharedAccessKey** olay hub'ı sayfasında Azure Portalı'nda. Ayrıntılı adımlar için bkz. [bağlantı dizesi alma](event-hubs-create.md#create-an-event-hubs-namespace).
4. Azure CLI gidin **istemci** klasör yolu. Düğüm paketleri yükleyin ve aşağıdaki komutları çalıştırarak projeyi derleyin:

    ```nodejs
    npm i
    npm run build
    ```
5. Aşağıdaki komutu çalıştırarak olayları göndermeye başlayın: 

    ```nodejs
    node dist/examples/simpleSender.js
    ```


## <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin 
Node.js kullanarak bir olay hub'ına olayları göndermek için örnek kod aşağıda verilmiştir. El ile sampleSender.js dosyası oluşturun ve bir olay hub'ına olayları göndermek için çalıştırın. 


```nodejs
const { EventHubClient, EventPosition } = require('@azure/event-hubs');

const client = EventHubClient.createFromConnectionString(process.env["EVENTHUB_CONNECTION_STRING"], process.env["EVENTHUB_NAME"]);

async function main() {
    // NOTE: For receiving events from Azure Stream Analytics, please send Events to an EventHub where the body is a JSON object/array.
    // const eventData = { body: { "message": "Hello World" } };
    const data = { body: "Hello World 1" };
    const delivery = await client.send(data);
    console.log("message sent successfully.");
}

main().catch((err) => {
    console.log(err);
});

```

Betiği çalıştırmadan önce ortam değişkenlerini ayarlamak unutmayın. Bu komut satırında aşağıdaki örnekte gösterilen şekilde yapılandırabilir veya kullanın [dotenv paket](https://www.npmjs.com/package/dotenv#dotenv). 

```
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [Node.js kullanarak olay alma](event-hubs-node-get-started-receive.md)
* [Github'da örnekleri](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

<!-- Links -->
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
