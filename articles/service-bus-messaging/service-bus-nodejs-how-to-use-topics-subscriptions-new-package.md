---
title: Node.js ile Azure Service Bus konu başlıklarını ve aboneliklerini kullanma | Microsoft Docs
description: Service Bus konuları ve Abonelikleri, Azure'da bir Node.js uygulamasından kullanmayı öğrenin.
services: service-bus-messaging
documentationcenter: nodejs
author: axisc
manager: timlt
editor: spelluru
ms.assetid: b9f5db85-7b6c-4cc7-bd2c-bd3087c99875
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: nodejs
ms.topic: article
ms.date: 04/15/2019
ms.author: aschhab
ms.openlocfilehash: 7686014adb989494e6df277de4137b76c3125696
ms.sourcegitcommit: cfbc8db6a3e3744062a533803e664ccee19f6d63
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2019
ms.locfileid: "65992126"
---
# <a name="how-to-use-service-bus-topics-and-subscriptions-with-nodejs-and-the-azureservice-bus-package"></a>Node.js ve azure/service-bus paketi ile Service Bus konu başlıklarını ve aboneliklerini kullanma
> [!div class="op_multi_selector" title1="Programming language" title2="Node.js pacakge"]
> - [(Node.js | azure sb)](service-bus-nodejs-how-to-use-topics-subscriptions.md)
> - [(Node.js | @azure/service-bus)](service-bus-nodejs-how-to-use-topics-subscriptions-new-package.md)

Bu öğreticide, bir Service Bus konusuna iletiler gönderir ve kullanarak yeni bir Service Bus aboneliğinden iletiler almak için bir Node.js program yazma konusunda bilgi edinin [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) paket. Bu paket daha hızlı kullanır [AMQP 1.0 protokol](service-bus-amqp-overview.md) ancak eski [azure sb](https://www.npmjs.com/package/azure-sb) kullanılan paket [hizmet veri yolu REST çalışma zamanı API'ları](/rest/api/servicebus/service-bus-runtime-rest). Örnekler, JavaScript dilinde yazılır.

## <a name="prerequisites"></a>Önkoşullar
- Azure aboneliği. Bu öğreticiyi tamamlamak için bir Azure hesabınızın olması gerekir. Etkinleştirebilir, [MSDN abone Avantajlarınızı](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/?WT.mc_id=A85619ABF) veya kaydolun bir [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A85619ABF).
- Bir konu ve çalışmak için abonelik yoksa, izleyeceğiniz adımlar [bir Service Bus'ı oluşturmak için Azure portalını kullanın konuları ve abonelikleri](service-bus-quickstart-topics-subscriptions-portal.md) bunları oluşturmak için makale. Service Bus örneğinizi ve oluşturduğunuz aboneliği ve konu adları için bağlantı dizesini not alın. Örnekler, bu değerleri kullanacağız.

> [!NOTE]
> - Bu öğreticide, kopyalama ve kullanarak çalıştırma örnekleri ile çalışır [Nodejs](https://nodejs.org/). Bir Node.js uygulaması oluşturma hakkında yönergeler için bkz: [oluşturun ve bir Azure Web sitesine bir Node.js uygulaması dağıtma](../app-service/app-service-web-get-started-nodejs.md), veya [Windows PowerShell kullanarak Node.js bulut hizmetini](../cloud-services/cloud-services-nodejs-develop-deploy-app.md).
> - Yeni [ @azure/service-bus ](https://www.npmjs.com/package/@azure/service-bus) paket oluşturma topcis ve abonelikleri desteklemez. Lütfen kullanın [ @azure/arm-servicebus ](https://www.npmjs.com/package/@azure/arm-servicebus) programlama yoluyla bunları oluşturmak istiyorsanız paketi.

### <a name="use-node-package-manager-npm-to-install-the-package"></a>Paketi yüklemek için Düğüm Paket Yöneticisi’ni (NPM) kullanma
Hizmet veri yolu için npm paketini yüklemek için olan bir komut istemi açın `npm` kendi yolunda örneklerinizi olması ve ardından bu komutu çalıştırmak istediğiniz klasöre dizini değiştirin.

```bash
npm install @azure/service-bus
```

## <a name="send-messages-to-a-topic"></a>Konu başlığına ileti gönderme
Service Bus ile etkileşim konu ile örnekleme başlatılır. [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfı ve örneği kullanarak [TopicClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/topicclient) sınıfı. Konu istemci aldıktan sonra bir gönderici oluşturun ve kullanın [Gönder](https://docs.microsoft.com/javascript/api/%40azure/service-bus/sender#send-sendablemessageinfo-) veya [sendBatch](https://docs.microsoft.com/javascript/api/@azure/service-bus/sender#sendbatch-sendablemessageinfo---) ileti göndermek için yöntemi.

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com/)
2. Adlı bir dosya oluşturun `send.js` Yapıştır kod içine aşağıdaki. Bu kod, Konunuza 10 iletileri gönderir.

    ```javascript
    const { ServiceBusClient } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const topicClient = sbClient.createTopicClient(topicName);
      const sender = topicClient.createSender();
      
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

            await topicClient.close();
          } finally {
            await sbClient.close();
          }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki kodda konunuzun adını ve bağlantı dizesini girin.
4. Ardından komutu çalıştırın `node send.js` bu dosyayı yürütmek için bir komut isteminde. 

Tebrikler! Yalnızca bir Service Bus kuyruğuna ileti gönderdi.

İletileriniz gibi bazı standart Özellikler `label` ve `messageId` gönderirken ayarlayabileceğiniz. Herhangi bir özel özelliği ayarlamak istiyorsanız, kullanın `userProperties`, anahtar-değer çiftleri özel veri tutabilen bir json nesnesi olduğu.

Service Bus konu başlıkları, [Standart katmanda](service-bus-premium-messaging.md) maksimum 256 KB ve [Premium katmanda](service-bus-premium-messaging.md) maksimum 1 MB ileti boyutunu destekler. Bir konu başlığında tutulan ileti sayısına bir sınır yoktur ancak konu başlığı tarafından tutulan iletilerin toplam boyutu sınırı yoktur. Bu konu başlığı boyutu, üst sınır 5 GB olacak şekilde oluşturulma zamanında belirlenir. Kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları](service-bus-quotas.md).

## <a name="receive-messages-from-a-subscription"></a>Abonelikten ileti alma
Service Bus ile etkileşim abonelik ile örnekleme başlatılır. [ServiceBusClient](https://docs.microsoft.com/javascript/api/@azure/service-bus/servicebusclient) sınıfı ve örneği kullanarak [SubscriptionClient](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient) sınıfı. Abonelik istemci aldıktan sonra bir alıcı oluşturun ve kullanın [receiveMessages](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#receivemessages-number--undefined---number-) veya [registerMessageHandler](https://docs.microsoft.com/javascript/api/%40azure/service-bus/receiver#registermessagehandler-onmessage--onerror--messagehandleroptions-) iletileri almak için yöntemi.

1. Sık kullandığınız düzenleyicinizi gibi açın [Visual Studio Code](https://code.visualstudio.com/)
2. Adlı bir dosya oluşturun `recieve.js` Yapıştır kod içine aşağıdaki. Bu kod, aboneliğinizden 10 iletileri almaya çalışacak. Aldığınız gerçek sayı, abonelik ve ağ gecikmesi ileti sayısını bağlıdır.

    ```javascript
    const { ServiceBusClient, ReceiveMode } = require("@azure/service-bus"); 
    
    // Define connection string and related Service Bus entity names here
    const connectionString = "";
    const topicName = ""; 
    const subscriptionName = ""; 
    
    async function main(){
      const sbClient = ServiceBusClient.createFromConnectionString(connectionString); 
      const subscriptionClient = sbClient.createSubscriptionClient(topicName, subscriptionName);
      const receiver = subscriptionClient.createReceiver(ReceiveMode.ReceiveAndDelete);
      
      try {
        const messages = await receiver.receiveMessages(10);
        console.log("Received messages:");
        console.log(messages.map(message => message.body));
        
        await subscriptionClient.close();
      } finally {
        await sbClient.close();
      }
    }
    
    main().catch((err) => {
      console.log("Error occurred: ", err);
    });
    ```
3. Yukarıdaki kodda, konu ve abonelik adını ve bağlantı dizesini girin.
4. Ardından komutu çalıştırın `node receiveMessages.js` bu dosyayı yürütmek için bir komut isteminde.

Tebrikler! Yalnızca bir Service Bus aboneliğinden iletileri aldı.

[CreateReceiver](https://docs.microsoft.com/javascript/api/%40azure/service-bus/subscriptionclient#createreceiver-receivemode-) yöntemi alır bir `ReceiveMode` bir sabit listesi değerlerine sahip olduğu [ReceiveAndDelete](message-transfers-locks-settlement.md#settling-receive-operations) ve [PeekLock](message-transfers-locks-settlement.md#settling-receive-operations). Unutmayın [iletilerinizi kapatma](message-transfers-locks-settlement.md#settling-receive-operations) kullanırsanız `PeekLock` herhangi birini kullanarak moduna `complete()`, `abandon()`, `defer()`, veya `deadletter()` ileti üzerinde yöntemleri.

## <a name="subscription-filters-and-actions"></a>Abonelik filtreleri ve eylemleri
Service Bus destekler [filtreleri ve eylemleri Aboneliklerde](topic-filters.md), olanak sağlayan bir abonelik için gelen iletiler için filtre ve bunların özelliklerini düzenlemek için.

Örneğini oluşturduktan sonra bir `SubscriptionClient` kullanabileceğiniz almak için yöntemleri, aşağıdaki ekleyin ve filtreleri ve eylemleri denetlemek için aboneliğe ilişkin kuralları kaldırın.

- getRules
- addRule
- removeRule

Her aboneliğin, tüm gelen iletilere izin verecek şekilde doğru bir filtre kullanır, varsayılan bir kural yok. Yeni bir kural eklediğinizde, varsayılan filtre çalışılabilmesi yeni kuralınıza filtrede için kaldırmayı unutmayın. Bir abonelikte hiçbir kural varsa, ileti alırsınız.

> [!NOTE]
> Service Bus kaynakları ile yönetebileceğiniz [hizmet veri yolu Gezgini](https://github.com/paolosalvatori/ServiceBusExplorer/). Hizmet veri yolu Gezgini, bir Service Bus ad alanınıza bağlanın ve mesajlaşma varlıkları kolay bir şekilde yönetmek kullanıcıların sağlar. Araç, içeri/dışarı aktarma işlevleri veya konu, kuyruklar, abonelikler, geçiş hizmetleri, bildirim hub'ları ve olay hub'ları test etme olanağı gibi gelişmiş özellikler sağlar. 

## <a name="next-steps"></a>Sonraki Adımlar
Daha fazla bilgi için aşağıdaki kaynaklara bakın.

- [Kuyruklar, konu başlıkları ve abonelikler](service-bus-queues-topics-subscriptions.md)
- Kullanıma alma diğer [Nodejs samples github'da Service Bus](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/servicebus/service-bus/samples/javascript)
- [Node.js Geliştirici Merkezi](https://azure.microsoft.com/develop/nodejs/)


