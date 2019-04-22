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
ms.openlocfilehash: f03bfde8f7ea37989756ad47678369e94b831438
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59677910"
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
- Kopya [örnek GitHub deposunda](https://github.com/Azure/azure-event-hubs-node) makinenizde. 


## <a name="send-events"></a>Olayları gönderme
Bu bölümde, olay hub'ına olayları gönderen bir Node.js uygulamasının nasıl oluşturulacağını gösterir. 

### <a name="install-nodejs-package"></a>Node.js paketini yükle
Node.js paketi için Azure Event Hubs makinenize yükleyin. 

```shell
npm install @azure/event-hubs
```

Önkoşul olarak belirtildiği gibi Git deposu kopyalanan yüklemediyseniz, indirme [örnek](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples) github'dan. 

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


### <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin 
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

## <a name="receive-events"></a>Olayları alma
Bu öğreticide, Azure'ı kullanarak bir olay hub'ından olayları almaya gösterilmektedir [EventProcessorHost](event-hubs-event-processor-host.md) bir Node.js uygulaması içinde. EventProcessorHost (EPH) bir olay hub'ı tüketici grubunu'ındaki tüm bölümler arasında alıcılar oluşturarak bir olay hub'ından etkili bir şekilde olayları alma yardımcı olur. Bu kontrol noktaları meta verileri Azure depolama blobu, düzenli aralıklarla alınan iletiler. Bu yaklaşım, daha sonraki bir zamanda kaldığı yerden gelen iletileri almaya devam etmek kolaylaştırır.

Bu Hızlı Başlangıç için kod kullanılabilir [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

### <a name="clone-the-git-repository"></a>Git deposunu kopyalayın
İndirin veya kopyalayın [örnek](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) github'dan. 

### <a name="install-the-eventprocessorhost"></a>EventProcessorHost yükleyin
Event Hubs modülü için EventProcessorHost yükleyin. 

```shell
npm install @azure/event-processor-host
```

### <a name="receive-events-using-eventprocessorhost"></a>EventProcessorHost kullanarak olay alma
Kopyalanmış SDK, Node.js kullanarak bir olay hub'ından olay alma işlemini gösteren birden fazla örnek içerir. Bu hızlı başlangıçta, kullandığınız **singleEPH.js** örnek. Alınan olayları gözlemektir için başka bir terminal açın ve kullanarak olayları gönderme [örnek gönderme](event-hubs-node-get-started-send.md).

1. Visual Studio Code projede açın. 
2. Adlı bir dosya oluşturun **.env** altında **İşlemci** klasör. Örnek ortam değişkenlerinden kopyalayıp **sample.env** kök klasöründe.
3. Olay hub'ı bağlantı dizesi, olay hub'ı adı ve depolama uç noktası yapılandırın. Olay hub'ından için bağlantı dizesini kopyalayabilirsiniz **bağlantı dizesi-birincil** anahtar altında **RootManageSharedAccessKey** olay hub'ı sayfasında Azure Portalı'nda. Ayrıntılı adımlar için bkz. [bağlantı dizesi alma](event-hubs-create.md#create-an-event-hubs-namespace).
4. Azure CLI gidin **İşlemci** klasör yolu. Düğüm paketleri yükleyin ve aşağıdaki komutları çalıştırarak projeyi derleyin:

    ```shell
    npm i
    npm run build
    ```
5. Olaylar, olay işlemcisi konağı ile aşağıdaki komutu çalıştırarak alırsınız:

    ```shell
    node dist/examples/singleEph.js
    ```

### <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin 
Node.js kullanarak bir olay hub'ından olaylarını almak için örnek kod aşağıda verilmiştir. El ile sampleEph.js dosyası oluşturun ve olay hub'ına olayları almak için çalıştırın. 

  ```javascript
  const { EventProcessorHost, delay } = require("@azure/event-processor-host");

  const path = process.env.EVENTHUB_NAME;
  const storageCS = process.env.STORAGE_CONNECTION_STRING;
  const ehCS = process.env.EVENTHUB_CONNECTION_STRING;
  const storageContainerName = "test-container";
  
  async function main() {
    // Create the Event Processor Host
    const eph = EventProcessorHost.createFromConnectionString(
      EventProcessorHost.createHostName("my-host"),
      storageCS,
      storageContainerName,
      ehCS,
      {
        eventHubPath: path,
        onEphError: (error) => {
          console.log("This handler will notify you of any internal errors that happen " +
          "during partition and lease management: %O", error);
        }
      }
    );
    let count = 0;
    // Message event handler
    const onMessage = async (context/*PartitionContext*/, data /*EventData*/) => {
      console.log(">>>>> Rx message from '%s': '%s'", context.partitionId, data.body);
      count++;
      // let us checkpoint every 100th message that is received across all the partitions.
      if (count % 100 === 0) {
        return await context.checkpoint();
      }
    };
    // Error event handler
    const onError = (error) => {
      console.log(">>>>> Received Error: %O", error);
    };
    // start the EPH
    await eph.start(onMessage, onError);
    // After some time let' say 2 minutes
    await delay(120000);
    // This will stop the EPH.
    await eph.stop();
  }
  
  main().catch((err) => {
    console.log(err);
  });
      
  ```

Betiği çalıştırmadan önce ortam değişkenlerini ayarlamak unutmayın. Bu komut satırında aşağıdaki örnekte gösterilen şekilde yapılandırabilir veya kullanın [dotenv paket](https://www.npmjs.com/package/dotenv#dotenv). 

```shell
// For windows
set EVENTHUB_CONNECTION_STRING="<your-connection-string>"
set EVENTHUB_NAME="<your-event-hub-name>"

// For linux or macos
export EVENTHUB_CONNECTION_STRING="<your-connection-string>"
export EVENTHUB_NAME="<your-event-hub-name>"
```

Daha fazla örnek bulabilirsiniz [burada](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples).


## <a name="next-steps"></a>Sonraki adımlar
Bu makaleleri okuyun:

- [EventProcessorHost](event-hubs-event-processor-host.md)
- [Özellikler ve Azure Event Hubs terminolojisinde](event-hubs-features.md)
- [Event Hubs ile ilgili SSS](event-hubs-faq.md)
- Event Hubs için diğer Node.js örneklerini gözden kontrol [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/client/examples/).
