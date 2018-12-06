---
title: Node.js kullanarak Azure Event Hubs için olayları gönderme | Microsoft Docs
description: Node.js kullanarak Event Hubs'a olay göndermeye başlayın.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.date: 12/05/2018
ms.author: shvija
ms.openlocfilehash: e64aab3aed582a60140ee1357e79ee5ee4a4cdf4
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/06/2018
ms.locfileid: "52965045"
---
# <a name="send-events-to-azure-event-hubs-using-nodejs"></a>Node.js kullanarak Azure Event Hubs için olayları gönderme

Azure Event Hubs saniyede milyonlarca olay alıp işleme kapasitesine sahip olan bir Büyük Veri akış platformu ve olay alma hizmetidir. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide, node.js'de yazılmış bir uygulamadan bir olay hub'ına olayları göndermek nasıl açıklar.

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Node.js sürümü 8.x ve daha yüksek. En son LTS sürümü [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (önerilir) veya diğer herhangi bir IDE

## <a name="create-an-event-hubs-namespace-and-an-event-hub"></a>Event Hubs ad alanı ve bir olay hub’ı oluşturma
İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için [bu makalede](event-hubs-create.md) verilen yordamı uygulayın, ardından bu öğreticide yer alan aşağıdaki adımlarla devam edin.

Makaledeki yönergeleri izleyerek olay hub'ı ad alanı için bağlantı dizesini alın: [bağlantı dizesi alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticide daha sonra'de bağlantı dizesini kullanın.

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
Bu hızlı başlangıçta, Node.js kullanarak bir olay hub'ına ileti gönderdiniz. Node.js kullanarak bir olay hub'ından olay alma konusunda bilgi almak için bkz: [event hub'dan - Node.js olayları alma](event-hubs-node-get-started-receive.md)

Event Hubs için diğer Node.js örneklerini gözden kontrol [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
