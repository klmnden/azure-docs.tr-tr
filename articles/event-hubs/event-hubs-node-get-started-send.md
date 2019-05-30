---
title: Gönder ve Node.js - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event Hubs'dan olayları gönderen bir Node.js uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: spelluru
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 04/15/2019
ms.author: spelluru
ms.openlocfilehash: e67be59e0ed78b2080986acb73a33fc87599c9d3
ms.sourcegitcommit: f6c85922b9e70bb83879e52c2aec6307c99a0cac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/11/2019
ms.locfileid: "65539339"
---
# <a name="send-events-to-or-receive-events-from-azure-event-hubs-using-nodejs"></a>Olayları göndermek veya Node.js kullanarak Azure Event Hubs'tan gelen olayları alma

Azure Event Hubs, bir büyük veri platformu ve alabilir, olay alma hizmetidir ve saniyede milyonlarca işlem akışı. Event Hubs dağıtılan yazılımlar ve cihazlar tarafından oluşturulan olayları, verileri ve telemetrileri işleyebilir ve depolayabilir. Bir olay hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve işlem grubu oluşturma/depolama bağdaştırıcıları kullanılarak dönüştürülüp depolanabilir. Olay Hub’larının ayrıntılı genel bakışı için bkz. [Olay Hub’larına genel bakış](event-hubs-about.md) ve [Olay Hub’ları özellikleri](event-hubs-features.md).

Bu öğreticide Node.js uygulamaları için olayları gönderme veya bir olay hub'ından olay alma oluşturmayı açıklar.

> [!NOTE]
> Bu hızlı başlangıcı [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client)’dan örnek olarak indirebilir, `EventHubConnectionString` ve `EventHubName` dizelerini olay hub’ınızdaki değerlerle değiştirebilir ve çalıştırabilirsiniz. Alternatif olarak bu öğreticideki adımları izleyerek kendi çözümünüzü de oluşturabilirsiniz.

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
- Node.js sürümü 8.x ve daha yüksek. En son LTS sürümü [ https://nodejs.org ](https://nodejs.org).
- Visual Studio Code (önerilir) veya diğer herhangi bir IDE
- **Bir Event Hubs ad alanı ve bir olay hub'ı oluşturma**. İlk adımda [Azure portalını](https://portal.azure.com) kullanarak Event Hubs türünde bir ad alanı oluşturun, ardından uygulamanızın olay hub’ı ile iletişim kurması için gereken yönetim kimlik bilgilerini edinin. Bir ad alanı ve olay hub'ı oluşturmak için verilen yordamı izleyin [bu makalede](event-hubs-create.md), sonra Bu öğreticide aşağıdaki adımlarla devam edin. Ardından, makaledeki yönergeleri izleyerek olay hub'ı ad alanı için bağlantı dizesini alın: [Bağlantı dizesini alma](event-hubs-get-connection-string.md#get-connection-string-from-the-portal). Bu öğreticide daha sonra'de bağlantı dizesini kullanın.


### <a name="install-npm-package"></a>Npm paketini yükleme
Yüklenecek [Event Hubs için npm paketini](https://www.npmjs.com/package/@azure/event-hubs), sahip bir komut istemi açın `npm` kendi yolunda dizin örneklerinizi olması ve ardından bu komutu çalıştırmak istediğiniz klasöre geçin.

```shell
npm install @azure/event-hubs
```

Yüklenecek [olay işleyicisi ana bilgisayarı için npm paketi](https://www.npmjs.com/package/@azure/event-processor-host)çalıştırın aşağıdaki komutu yerine

```shell
npm install @azure/event-processor-host
```

## <a name="send-events"></a>Olayları gönderme

Bu bölümde, olay hub'ına olayları gönderen bir Node.js uygulamasının nasıl oluşturulacağını gösterir. 

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com). 
2. Adlı bir dosya oluşturun `send.js` Yapıştır kod içine aşağıdaki.
    ```javascript
    const { EventHubClient } = require("@azure/event-hubs");

    // Define connection string and the name of the Event Hub
    const connectionString = "";
    const eventHubsName = "";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);

      for (let i = 0; i < 100; i++) {
        const eventData = {body: `Event ${i}`};
        console.log(`Sending message: ${eventData.body}`);
        await client.send(eventData);
      }

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki kodda bağlantı dizesini ve olay Hub'ınızın adını girin
4. Ardından komutu çalıştırın `node send.js` bu dosyayı yürütmek için bir komut isteminde. Bu 100 olayları olay Hub'ınıza gönderir

Tebrikler! Olayları bir event hub'ına gönderdiniz.


## <a name="receive-events"></a>Olayları alma

Bu bölümde, olayları bir event hub'ındaki varsayılan tüketici grubunun tek bir bölüm aldığında bir Node.js uygulamasının nasıl oluşturulacağını gösterir. 

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com). 
2. Adlı bir dosya oluşturun `receive.js` Yapıştır kod içine aşağıdaki.
    ```javascript
    const { EventHubClient, delay } = require("@azure/event-hubs");

    // Define connection string and related Event Hubs entity name here
    const connectionString = "";
    const eventHubsName = "";

    async function main() {
      const client = EventHubClient.createFromConnectionString(connectionString, eventHubsName);
      const allPartitionIds = await client.getPartitionIds();
      const firstPartitionId = allPartitionIds[0];

      const receiveHandler = client.receive(firstPartitionId, eventData => {
        console.log(`Received message: ${eventData.body} from partition ${firstPartitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await receiveHandler.stop();

      await client.close();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki kodda, bağlantı dizesini ve olay Hub'ınızın adını girin.
4. Ardından komutu çalıştırın `node receive.js` bu dosyayı yürütmek için bir komut isteminde. Bu bölümler, varsayılan bir tüketici grubu olay hub'ınızdaki birinden olayları alacaksınız

Tebrikler! Şimdi, olayları olay hub'ından aldınız.

## <a name="receive-events-using-event-processor-host"></a>Olay işlemcisi konağı kullanarak olay alma

Bu bölümde, Azure'ı kullanarak bir olay hub'ından olay alma işlemi gösterilmektedir [EventProcessorHost](event-hubs-event-processor-host.md) bir Node.js uygulaması içinde. EventProcessorHost (EPH) bir olay hub'ı tüketici grubunu'ındaki tüm bölümler arasında alıcılar oluşturarak bir olay hub'ından etkili bir şekilde olayları alma yardımcı olur. Bu kontrol noktaları meta verileri Azure depolama blobu, düzenli aralıklarla alınan iletiler. Bu yaklaşım, daha sonraki bir zamanda kaldığı yerden gelen iletileri almaya devam etmek kolaylaştırır.

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com). 
2. Adlı bir dosya oluşturun `receiveAll.js` Yapıştır kod içine aşağıdaki.
    ```javascript
    const { EventProcessorHost, delay } = require("@azure/event-processor-host");

    // Define connection string and related Event Hubs entity name here
    const eventHubConnectionString = "";
    const eventHubName = "";
    const storageConnectionString = "";

    async function main() {
      const eph = EventProcessorHost.createFromConnectionString(
        "my-eph",
        storageConnectionString,
        "my-storage-container-name",
        eventHubConnectionString,
        {
          eventHubPath: eventHubName,
          onEphError: (error) => {
            console.log("[%s] Error: %O", error);
          }
        }
      );


      eph.start((context, eventData) => {
        console.log(`Received message: ${eventData.body} from partition ${context.partitionId}`);
      }, error => {
        console.log('Error when receiving message: ', error)
      });

      // Sleep for a while before stopping the receive operation.
      await delay(15000);
      await eph.stop();
    }

    main().catch(err => {
      console.log("Error occurred: ", err);
    });

    ```
3. Yukarıdaki kodda bağlantı dizesi ile birlikte bağlantı dizesini ve olay Hub'ınızın adını bir Azure Blob Depolama için girin
4. Ardından komutu çalıştırın `node receiveAll.js` bu dosyayı yürütmek için bir komut isteminde.

Tebrikler! Şimdi, olayları olay işlemcisi konağı kullanarak olay hub'ından aldınız. Bu, varsayılan bir tüketici grubu, olay Hub'ındaki tüm bölümler gelen olayları alacaksınız

## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Özellikler ve Azure Event Hubs terminolojisinde](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)
- Diğer Node.js Örnekleri kullanıma [Event Hubs](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-hubs/samples) ve [Event Processor Host](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/eventhub/event-processor-host/samples) github'da
