---
title: Node.js'de - service/azure-bus Azure Service Bus kuyruklarını kullanma | Microsoft Docs
description: Azure'da bir Node.js uygulamasından Service Bus kuyruklarını kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: a87a00f9-9aba-4c49-a0df-f900a8b67b3f
ms.service: service-bus-messaging
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/10/2019
ms.author: aschhab
ms.openlocfilehash: 7aacefde9c037fcce64d9256e35082eb04e0a2f3
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65988360"
---
# <a name="how-to-use-service-bus-queues-with-nodejs-and-the-azureservice-bus-package"></a>Node.js ve azure/service-bus paketi ile Service Bus kuyruklarını kullanma
> [!div class="op_multi_selector" title1="Programlama dili" title2="Node.js paketi"]
> - [(Node.js | azure sb)](service-bus-nodejs-how-to-use-queues.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-queues-new-package.md)

Bu öğreticide, bir Nodejs program kullanarak yeni bir Service Bus kuyruğundaki iletileri alıp ileti göndermek için yazma konusunda bilgi edinin [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) paket. Bu paket daha hızlı kullanır [AMQP 1.0 protokol](service-bus-amqp-overview.md) ancak eski [azure sb](https://www.npmjs.com/package/azure-sb) kullanılan paket [hizmet veri yolu REST çalışma zamanı API'ları](/rest/api/servicebus/service-bus-runtime-rest). Örnekler, JavaScript dilinde yazılır.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Çalışmak için bir kuyruk yoksa, izleyeceğiniz adımlar [Service Bus kuyruğuna oluşturmak için Azure portalını kullanın](service-bus-quickstart-portal.md) makalenin bir kuyruk oluşturun. Service Bus örneğinizi ve oluşturduğunuz Kuyruğun adı için bağlantı dizesini not alın. Örnekler, bu değerleri kullanacağız.

> [!NOTE]
> - Bu öğreticide, kopyalama ve kullanarak çalıştırma örnekleri ile çalışır [Nodejs](https://nodejs.org/). Bir Node.js uygulaması oluşturma hakkında yönergeler için bkz: [oluşturun ve bir Azure Web sitesine bir Node.js uygulaması dağıtma](../app-service/app-service-web-get-started-nodejs.md), veya [Windows PowerShell kullanarak Node.js bulut hizmeti](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - Yeni [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) paket henüz kuyrukları oluşturulmasını desteklemiyor. Lütfen kullanın [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) programlama yoluyla bunları oluşturmak istiyorsanız paketi.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için Düğüm Paket Yöneticisi’ni (NPM) kullanma
Hizmet veri yolu için npm paketini yüklemek için olan bir komut istemi açın `npm` kendi yolunda örneklerinizi olması ve ardından bu komutu çalıştırmak istediğiniz klasöre dizini değiştirin.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-queue"></a>Kuyruğa ileti gönderme
Bir Service Bus ile etkileşim kuyruk ile örnekleme başlatılır. [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfı ve örneği kullanarak [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) sınıfı. Kuyruk istemci aldıktan sonra bir gönderici oluşturup ya da kullanabilirsiniz [Gönder](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) veya [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) ileti göndermek için yöntemi.

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com/)
2. Adlı bir dosya oluşturun `send.js` Yapıştır kod içine aşağıdaki. Bu kod, 10 ileti kuyruğuna gönderir.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const sender = queueClient.createSender();
      
      try {
        for (let i = 0; i < 10; i++) {
          const message= {
            body: `Hello world! ${i}`,
            label: `test`,
            userProperties: {
                myCustomPropertyName: `my custom property value ${i}`
           }
          };
          console.log(`Sending message: ${message.body}`);
          await sender.send(message);
        }
        
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki kodda kuyruğunuzun adı ve bağlantı dizesini girin.
4. Ardından komutu çalıştırın `node send.js` bu dosyayı yürütmek için bir komut isteminde.

Tebrikler! Yalnızca bir Service Bus kuyruğuna ileti gönderdi.

İletileriniz gibi bazı standart Özellikler `label` ve `messageId` gönderirken ayarlayabileceğiniz. Herhangi bir özel özelliği ayarlamak istiyorsanız, kullanın `userProperties`, anahtar-değer çiftleri özel veri tutabilen bir json nesnesi olduğu.

Service Bus kuyrukları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Kuyrukta tutulan ileti sayısına bir sınır yoktur ancak kuyruk tarafından tutulan iletilerin toplam boyutu bir sınır yoktur. Bu kuyruk boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları](service-bus-quotas.md).

## <a name="receive-messages-from-a-queue"></a>Bir kuyruktan ileti alma
Bir Service Bus ile etkileşim kuyruk ile örnekleme başlatılır. [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfı ve örneği kullanarak [QueueClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient) sınıfı. Kuyruk istemci aldıktan sonra bir alıcı oluşturun ve kullanın [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) veya [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) iletileri almak için yöntemi.

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com/)
2. Adlı bir dosya oluşturun `recieve.js` Yapıştır kod içine aşağıdaki. Bu kod, 10 ileti kuyruktan alma dener. Aldığınız gerçek sayı, sıra ve ağ gecikme süresi iletileri sayısına bağlıdır.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const queueName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const queueClient = sbClient.createQueueClient(queueName);
      const receiver = queueClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      try {
        const messages = await receiver.receiveMessages(10)
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
         
        await queueClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki kodda kuyruğunuzun adı ve bağlantı dizesini girin.
4. Ardından komutu çalıştırın `node receiveMessages.js` bu dosyayı yürütmek için bir komut isteminde.

Tebrikler! Yalnızca bir Service Bus kuyruğundaki iletileri aldı.

[CreateReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/queueclient#createreceiver-receivemode-) yöntemi alır bir `ReceiveMode` bir sabit listesi değerlerine sahip olduğu [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) ve [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). Unutmayın [iletilerinizi kapatma](message-transfers-locks-settlement.md#settling-receive-operations) kullanırsanız `PeekLock` herhangi birini kullanarak moduna `complete()`, `abandon()`, `defer()`, veya `deadletter()` ileti üzerinde yöntemleri.

> [!NOTE]
> Service Bus kaynakları ile yönetebileceğiniz [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer/). Hizmet veri yolu Gezgini, bir Service Bus ad alanınıza bağlanın ve mesajlaşma varlıkları kolay bir şekilde yönetmek kullanıcıların sağlar. Araç, içeri/dışarı aktarma işlevleri veya konu, kuyruklar, abonelikler, geçiş hizmetleri, bildirim hub'ları ve olay hub'ları test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın.
- [Kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md)
- Kullanıma alma diğer [Nodejs samples github'da Service Bus](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)

