---
title: Azure Service Bus kuyrukları, konular ve abonelikler Mesajlaşma genel bakış | Microsoft Docs
description: Service Bus Mesajlaşma varlıkları genel bakış.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.service: service-bus-messaging
ms.topic: article
ms.date: 09/18/2018
ms.author: aschhab
ms.openlocfilehash: 7cacabf4f171189810e943043b5513e20113d962
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "62125823"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus kuyrukları, konu başlıkları ve abonelikleri

Microsoft Azure Service Bus, güvenilir ileti sıraya alma gibi bulut tabanlı, iletiye yönelik ara yazılım teknolojilerini ve dayanıklı Yayımla/abone olma Mesajlaşma, bir kümesini destekler. Bu "aracılı" Mesajlaşma işlevleri, ardından destek Yayımla-abone ol Mesajlaşma özelliklerini, zamana bağlı ayırma ve Yük Dengeleme senaryolarıyla Service Bus Mesajlaşma iş yükünü kullanarak ayrılmış olarak düşünülebilir. Ayrılmış iletişim birçok avantaj sunar. Örneğin, sunucular ve istemciler gerektiğinde bağlantı kurabilir ve kendi işlemlerini zaman uyumsuz olarak gerçekleştirebilir.

Service Bus Mesajlaşma özelliklerini temel oluşturan ileti kuyrukları, konular ve abonelikler ve kurallar/eylemlerden varlıklardır.

## <a name="queues"></a>Kuyruklar

Kuyrukları *ilk giren, ilk kullanıma* (FIFO) bir veya birden çok rakip tüketiciye ileti teslimi. Diğer bir deyişle, alıcılar genellikle iletileri almak ve hangi kuyruğa eklenmiş olan ve yalnızca bir ileti tüketicisi alır ve her iletiyi işleyen sırada işlemek. Kuyrukları kullanmanın önemli bir avantajı, uygulama bileşenlerinin "zamana bağlı ayırma"'i elde etmektir. İletileri arızaya kuyrukta depolandığından diğer bir deyişle, üreticiler (göndericiler) ve tüketicilerin (Alıcılar) aynı anda ileti alma ve gönderme gerekmez. Ayrıca, işlemek ve ileti göndermek devam etmek için bir yanıt için beklemek üretici yok.

", Üreticilerin ve tüketicilerin iletileri farklı hızlarda gönderip sağlayan Yük Dengeleme" bir avantaj ise. Birçok uygulamada, sistem yükü, zaman içinde değişir; Ancak, her bir birimi, iş için gereken işleme süresi genellikle sabittir. Arasında bağlantı kurmak ileti üreticileri ve tüketicileri bir kuyruk aracılığıyla, kullanıcı uygulamanın yalnızca ortalama yük yoğun yük yerine işleyebilmesi için hazırlanması olduğunu anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu özellik uygulama yükünü sunmak için gerekli altyapı miktarını ilgili money doğrudan kaydeder. Yük arttıkça kuyruktan okunmak üzere daha fazla çalışan işlemi eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca, işleme gücü ile ilgili olarak çalışan bilgisayarlar farklılık gösterse bile, iletileri kendi maksimum hızında çekme olarak çalışan bilgisayarlar için en iyi kullanımı bu çekme tabanlı yük dengeleme sağlar. Bu düzen, genellikle "tüketici rakip" düzeni olarak adlandırılır.

Kuyruğa ileti üreticileri ve tüketicileri arasında ara kullanarak devredilen gevşek bağ modelini bileşenleri arasındaki sağlar. Üreticiler ve tüketiciler birbirinden farkında olmadığından, bir tüketici üretici herhangi bir etkisi olmadan yükseltilebilir.

### <a name="create-queues"></a>Kuyruk oluşturma

Kuyrukları kullanarak oluşturduğunuz [Azure portalında](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md), veya [Resource Manager şablonları](service-bus-resource-manager-namespace-queue.md). Daha sonra gönderebilir ve kullanarak ileti alma bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesne.

Hızlıca bir kuyruk oluşturun, sonra göndermek ve ve kuyruktan ileti almak nasıl öğrenmek için bkz. [hızlı başlangıçlar](service-bus-quickstart-portal.md) her yöntem için. Kuyrukları kullanma hakkında daha ayrıntılı bir öğretici için bkz: [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md).

Çalışma örnek için bkz: [BasicSendReceiveUsingQueueClient örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) GitHub üzerinde.

### <a name="receive-modes"></a>Modları alma

Service Bus iletileri aldığı iki farklı mod belirtebilirsiniz: *ReceiveAndDelete* veya *PeekLock*. İçinde [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modu alma işlemi tek; diğer bir deyişle, Service Bus bir istek aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** modu en basit modeldir ve içinde uygulama tolere edebilen bir hata oluşursa bir iletiyi işlememeye izin senaryolarda en iyi şekilde çalışır. Bu senaryo anlamak için hangi tüketici alma isteği bildirdiğini ve ardından işlenmeden önce kilitleniyor bir senaryo düşünün. Service Bus iletisi, uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında, Tüketilmekte olan olarak işaretlendikleri çökmenin öncesinde kullanılan iletiyi atlamış olur.

İçinde [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modu alma işlemi olur iki aşamalı hangi, atlanan iletilere veremeyen uygulamaları desteklemenin mümkün kılar. Service Bus bir istek aldığında, kullanılacak sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve ardından uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya güvenilir bir şekilde işlemek üzere depolar sonra) çağırarak alma işleminin ikinci aşamasını tamamlar [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) alınan iletide. Service Bus gördüğünde **CompleteAsync** çağrı, iletiyi kullanılıyor olarak işaretler.

Uygulama herhangi bir nedenden dolayı iletiyi işleyemiyor ise çağırabilirsiniz [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) yöntemi alınan iletide (yerine [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Bu yöntem, Service Bus'ın iletinin kilidini açmasına ve iletiyi aynı tüketici veya başka bir Yarışan tüketici tarafından tekrar alınabilir hale sağlar. İkincisi, kilit ile ilişkili bir zaman aşımı ve (örneğin, uygulama çökerse) Service Bus iletinin kilidini açar ve alınabilmesini halinde kilit zaman aşımı dolmadan önce iletiyi işlemek uygulama başarısız olursa kullanılabilir olması yeniden alınan) temelde gerçekleştiren bir [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) işlemi varsayılan olarak).

Uygulama ileti, ancak önce çökmesi durumunda, **CompleteAsync** isteği bildirilmeden, yeniden başlatıldığında ileti uygulamaya yeniden teslim. Bu işlem genellikle çağrılırken *en az bir kez* işleme; diğer bir deyişle, her ileti en az bir kez işlenir. Ancak belirli durumlarda aynı ileti yeniden teslim edilebilir. Yinelemeleri algılamak için uygulamanın ek mantık Required yinelenen işleme, senaryo kaldıramıyorsa, ulaşılabilecek temel [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) arasında sabit mi kaldığını ileti özelliği teslim denemesi. Bu özellik olarak da bilinen *tam bir kez* işleniyor.

## <a name="topics-and-subscriptions"></a>Konular ve abonelikler

Kuyruklar, her ileti tek bir tüketici tarafından işlenir aksine *konuları* ve *abonelikleri* bir-çok form iletişim, sağladığınız bir *Yayımlama/abonelik* deseni. Ölçekleme alıcı çok sayıda kullanışlı, yayınlanan tüm iletilerin konuya kaydedilen her abonelik için kullanılabilir. İletiler konu başlığına gönderilen ve bir veya daha fazla ilişkili Abonelikleri, abonelik başına temelinde ayarlanabilir filtre kuralları bağlı olarak teslim edilir. Abonelikler, almak istediğiniz iletileri kısıtlamak için ek filtreler kullanabilirsiniz. Gönderilen iletileri bir konuya aynı şekilde, bir kuyruğa gönderilir ancak iletilerin değil alındığı konu başlığından doğrudan. Bunun yerine, bunlar aboneliklerden alınır. Konu aboneliği konu başlığına gönderilen iletilerin kopyaların alan sanal kuyruğa benzer. İletiler bir abonelikten bir kuyruktan alınan olduğu gibi aynı şekilde alınır.

Karşılaştırma, yoluyla doğrudan bir konuya bir kuyruğa ileti gönderme işlevlerini eşler ve bir abonelik için ileti alma işlevselliğini eşler. Abonelikleri kuyruklar ile ilgili bu bölümde daha önce açıklanan aynı desenleri desteklemesini yanı sıra, bu özellik anlamına gelir: Yarışan tüketici, zamana bağlı ayırma, Yük Dengeleme ve Yük Dengeleme.

### <a name="create-topics-and-subscriptions"></a>Konu başlıklarını ve abonelikleri oluşturma

Bir konu oluşturmak önceki bölümde açıklandığı gibi bir kuyruk oluşturma işlemiyle benzerdir. Ardından kullanarak ileti gönderme [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) sınıfı. İleti almak için konuya bir veya daha fazla abonelik oluşturun. Benzer şekilde, kuyruklar, ileti kullanarak bir abonelik alındığında bir [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) yerine Nesne bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesne. Konu adı, abonelik ve (isteğe bağlı) alma modu adını parametre olarak geçirmeyi abonelik istemcisi oluşturmak.

Bir tam örnek, çalışıp [BasicSendReceiveUsingTopicSubscriptionClient örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) GitHub üzerinde.

### <a name="rules-and-actions"></a>Kurallar ve eylemler

Birçok senaryoda belirli özelliklere sahip iletileri farklı yollarla işlenmesi gerekir. Bu işlemeyi etkinleştirmek için istenen özellikleri ve sonra da bu özellikler için bazı değişiklikleri gerçekleştirin iletilerini bulmak için abonelik yapılandırabilirsiniz. Service Bus abonelikleri konu başlığına gönderilen tüm iletilerin görmenize rağmen bir alt kümesini iletiler aboneliğin sanal kuyruğuna yalnızca kopyalayabilirsiniz. Bu filtre, abonelik filtreleri kullanılarak gerçekleştirilir. Bu tür değişiklikler adlı *filtre eylemlerini*. Bir abonelik oluşturulurken, hem Sistem özellikleri iletinin özellikleri çalışır bir filtre ifadesi sağlayabilirsiniz (örneğin, **etiket**) ve özel uygulama özelliklerini (örneğin,  **StoreName**.) SQL filtre ifadesini, bu durumda isteğe bağlıdır; bir SQL filtresi ifadesi, bu abonelik için tüm iletiler bir abonelikte tanımlanan herhangi bir filtre işlem gerçekleştirilir.

Bir tam örnek, çalışıp [TopicSubscriptionWithRuleOperationsSample örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) GitHub üzerinde.

Olası filtre değerleri hakkında daha fazla bilgi için belgelere bakın [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) ve [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) sınıfları.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve Service Bus mesajlaşmasını kullanma örnekleri için aşağıdaki konulara bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Hızlı Başlangıç: Azure portalı ve .NET kullanarak ileti alma ve gönderme](service-bus-quickstart-portal.md)
* [Öğretici: Azure portalı ve konular/abonelikler aracılığıyla Envanter güncelleştirme](service-bus-tutorial-topics-subscriptions-portal.md)


