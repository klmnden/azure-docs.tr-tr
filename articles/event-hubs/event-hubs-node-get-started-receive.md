---
title: Node.js - Azure Event Hubs kullanarak olay alma | Microsoft Docs
description: Bu makalede, Azure Event Hubs'tan gelen olayları alır bir Node.js uygulaması oluşturmak için bir kılavuz sağlar.
services: event-hubs
author: ShubhaVijayasarathy
manager: kamalb
ms.service: event-hubs
ms.workload: core
ms.topic: article
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 55e4ce4a59a498c5c22fe6e701e3140aa65e7a10
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55895253"
---
# <a name="receive-events-from-azure-event-hubs-using-nodejs"></a>Node.js kullanarak Azure Event Hubs'tan gelen olayları alma

Azure Event Hubs, milyonlarca işleyebilen ileri düzeyde ölçeklenebilir bir olay yönetim sistemi saniyede olayları, işlemek ve çok büyük miktardaki verileri çözümlemek uygulamaları etkinleştirme bağlı cihazlarınız ve diğer sistemleri tarafından üretilen. Bir olay hub'ına toplandıktan sonra almak ve işlem içi işleyicileri kullanarak olayları işleme veya diğer analiz sistemleri ileterek. İşlenmeden önce olay verilerini Azure Depolama'da veya Azure Data Lake Store yakalayabilirsiniz.  

Event Hubs hakkında daha fazla bilgi için bkz: [Event Hubs'a genel bakış](event-hubs-about.md).

Bu öğreticide, Azure'ı kullanarak bir olay hub'ından olayları almaya gösterilmektedir [EventProcessorHost](event-hubs-event-processor-host.md) bir Node.js uygulaması içinde. EventProcessorHost (EPH) bir olay hub'ı tüketici grubunu'ındaki tüm bölümler arasında alıcılar oluşturarak bir olay hub'ından etkili bir şekilde olayları alma yardımcı olur. Bu kontrol noktaları meta verileri Azure depolama blobu, düzenli aralıklarla alınan iletiler. Bu yaklaşım, daha sonraki bir zamanda kaldığı yerden gelen iletileri almaya devam etmek kolaylaştırır.

Bu Hızlı Başlangıç için kod kullanılabilir [GitHub](https://github.com/Azure/azure-event-hubs-node/tree/master/processor).

> [!NOTE]
>  Node.js kullanarak Event Hubs için olayları göndermek için bu makaleye bakın: [Node.js kullanarak Azure Event Hubs için olayları gönderme](event-hubs-node-get-started-send.md). 

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi tamamlamak için aşağıdaki önkoşulları karşılamanız gerekir:

- Node.js sürümü 8.x ve daha yüksek. En son LTS sürümü [ https://nodejs.org ](https://nodejs.org). Node.js LTS sürümünü daha eski kullanmayın. 
- Etkin bir Azure hesabı. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][] oluşturun.

## <a name="create-a-namespace-and-event-hub"></a>Ad alanı ve olay hub'ı oluşturma
İlk adım, bir olay hub'ı ile bir Event Hubs ad alanı oluşturmak için Azure portalı kullanmaktır. Mevcut bir yoksa, yönergeleri izleyerek bu varlıkları oluşturabilirsiniz [bir Event Hubs ad alanı ve Azure portalını kullanarak bir olay hub'ı oluşturma](event-hubs-create.md).

## <a name="create-a-storage-account-and-container"></a>Bir depolama hesabı ve kapsayıcı oluşturma
Eventprocessorhost'u kullanmak için bir Azure depolama hesabı olmalıdır. Durum bilgisi gibi bölümleri ve kontrol noktaları kiraları olay akışında paylaşılır kullanarak bir Azure depolama kapsayıcısı alıcılar arasında. ' Ndaki yönergeleri takip ederek, bir Azure depolama hesabı oluşturabilirsiniz [bu makalede](../storage/common/storage-quickstart-create-account.md).

## <a name="clone-the-git-repository"></a>Git deposunu kopyalayın
İndirin veya kopyalayın [örnek](https://github.com/Azure/azure-event-hubs-node/tree/master/processor/examples/) github'dan. 

## <a name="install-the-eventprocessorhost"></a>EventProcessorHost yükleyin
Event Hubs modülü için EventProcessorHost yükleyin. 

```shell
npm install @azure/event-processor-host
```

## <a name="receive-events-using-eventprocessorhost"></a>EventProcessorHost kullanarak olay alma
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

## <a name="review-the-sample-code"></a>Örnek kodu gözden geçirin 
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

Bu hızlı başlangıçta, bir olay hub'ından iletiler alan bir Node.js uygulaması oluşturdunuz. Node.js kullanarak bir olay hub'ına olay gönderme hakkında bilgi edinmek için bkz: [olayları event hub'dan - Node.js Gönder](event-hubs-node-get-started-send.md).

<!-- Links -->
[ücretsiz bir hesap]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
