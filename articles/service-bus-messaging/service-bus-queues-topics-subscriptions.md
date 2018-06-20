---
title: Azure Service Bus kuyrukları, konu başlıkları ve abonelikleri Mesajlaşma genel bakış | Microsoft Docs
description: Service Bus Mesajlaşma genel bakış.
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
ms.service: service-bus-messaging
ms.topic: article
ms.date: 06/18/2018
ms.author: sethm
ms.openlocfilehash: 424004a2a39bd0d05bce515dc17685e60f7a0c9b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/19/2018
ms.locfileid: "36231585"
---
# <a name="service-bus-queues-topics-and-subscriptions"></a>Service Bus kuyrukları, konu başlıkları ve abonelikleri

Microsoft Azure Service Bus güvenilir message queuing bulut tabanlı, ileti odaklı Ara teknolojilerini ve dayanıklı yayımlama/Mesajlaşma abonelik, bir kümesini destekler. Bu "aracılı" Mesajlaşma işlevleri, ardından destek yayımlama-abone olma Mesajlaşma özellikleri, zamana bağlı ayırma ve Yük Dengeleme Service Bus Mesajlaşma iş yükü kullanarak ayrılmış olarak düşünülebilir. Ayrılmış iletişim birçok avantaj sunar. Örneğin, sunucular ve istemciler gerektiğinde bağlantı kurabilir ve kendi işlemlerini zaman uyumsuz olarak gerçekleştirebilir.

Hizmet veri yolundaki özünü Mesajlaşma özelliklerini Mesajlaşma varlıkları kuyruklar, konuları ve abonelikleri ve kuralları/eylemleri ' dir.

## <a name="queues"></a>Kuyruklar

Kuyruklar teklif *ilk gelen, giden ilk* (FIFO yöntemine göre) ileti teslimi bir veya birden çok rakip tüketiciye. Diğer bir deyişle, alıcılar genellikle iletileri almak ve hangi kuyruğa eklendikleri ve tek bir ileti tüketicisi alır ve her iletinin işler sırayla işlemek. Kuyrukları kullanmanın önemli bir avantajı "zamana bağlı ayırma" uygulama bileşenlerinin elde etmektir. İletileri işlemi kuyrukta depolandığından diğer bir deyişle, üreticiler (göndericiler) ve tüketicilerin (Alıcılar) aynı anda ileti alma ve gönderme gerekmez. Ayrıca, üretici işlemek ve iletileri göndermek devam etmek için bir yanıt beklerken yok.

", Üreticilerin ve tüketicilerin iletileri farklı hızlarda gönderip almasına sağlayan Yük Dengeleme" bir avantaj ise. Birçok uygulamada, sistem yükünün zaman içinde değişir; Bununla birlikte, her iş birimi için gereken işleme süresi genellikle sabittir. Aracılığıyla ileti üreticileri ve tüketicileri bir kuyruk, kullanıcı uygulamanın yalnızca ortalama yük yoğun yük yerine işlemek için kullanılacak olduğunu anlamına gelir. Gelen yük hacmi değiştikçe kuyruğun derinliği artar ve daralır. Bu özellik, doğrudan para uygulama yükünü sunmak için gereken altyapı miktarı ile kaydeder. Yük arttıkça kuyruktan okunmak üzere daha fazla çalışan işlemi eklenebilir. Her ileti yalnızca bir çalışan işlemi tarafından işlenir. Ayrıca, çalışan bilgisayarlar işleme gücünü göre farklılık gösterse bile bunların iletileri kendi maksimum hızında çekme olarak çalışan bilgisayarlar için en iyi kullanımı bu çekme tabanlı yük dengeleme sağlar. Bu desen genellikle "tüketici rekabet" düzeni olarak adlandırılır.

İleti üreticileri ve tüketicileri arasında ara için kuyrukları kullanma bileşenleri arasındaki yapısında bir gevşek bağlantı sağlar. Üreticileri ve tüketicileri birbirinden farkında değildir çünkü bir tüketici üretici üzerinde hiçbir etkisi olmadan yükseltilebilir.

### <a name="create-queues"></a>Sıra oluşturma

Kullanarak sıraları oluşturma [Azure portal](service-bus-quickstart-portal.md), [PowerShell](service-bus-quickstart-powershell.md), [CLI](service-bus-quickstart-cli.md), veya [Resource Manager şablonları](service-bus-resource-manager-namespace-queue.md). Ardından gönderip iletileri kullanarak bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesnesi. 

Hızlıca bir kuyruk oluşturun ardından gönderip ve kuyruktan ileti öğrenmek için bkz: [quickstarts](service-bus-quickstart-portal.md) her bir yöntemin. Kuyrukları kullanma hakkında daha ayrıntılı bir öğretici için bkz: [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md). 

Çalışma örnek için bkz: [BasicSendReceiveUsingQueueClient örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingQueueClient) github'da.

### <a name="receive-modes"></a>Modları alma

Hizmet veri yolu iletileri alır iki farklı mod belirtebilirsiniz: *ReceiveAndDelete* veya *PeekLock*. İçinde [ReceiveAndDelete](/dotnet/api/microsoft.azure.servicebus.receivemode) modu, alma işlemi tek; diğer bir deyişle, Service Bus isteği aldığında, iletiyi kullanılıyor olarak işaretler ve uygulamaya döndürür. **ReceiveAndDelete** modu en basit modeldir ve, uygulama tolerans bir hata oluşursa bir ileti işlenmiyor senaryolarda en iyi şekilde çalışır. Bu senaryo anlamak için tüketici alma isteği bildirdiğini ve isteğin işlenmeden çöktüğünü bir senaryo düşünün. Service Bus iletiyi uygulama yeniden başlatılıp iletileri tekrar kullanmaya başladığında olduğunda, kullanılıyor olarak işaretler çünkü çökmenin öncesinde kullanılan iletiyi atlamış olur.

İçinde [PeekLock](/dotnet/api/microsoft.azure.servicebus.receivemode) modu, alma işlemi hale iki aşamalı hangi, iletilere veremeyen uygulamaları desteklemenin mümkün kılar. Service Bus isteği aldığında, kullanılacak sonraki iletiyi bulur, diğer tüketicilerin iletiyi almasını engellemek için kilitler ve uygulamaya döndürür. Uygulama iletiyi işlemeyi tamamladıktan sonra (veya sonra işlemek için depoladıktan sonra), çağırarak alma işleminin ikinci aşamasını tamamlar [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync) alınan iletide. Hizmet veri yolu gördüğünde **CompleteAsync** çağrısı, iletiyi kullanılıyor olarak işaretler.

Uygulama herhangi bir nedenden dolayı iletisi işleyemedi ise çağırabilirsiniz [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) alınan iletide yöntemi (yerine [CompleteAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.completeasync)). Bu yöntem, iletinin kilidini açmak ve aynı tüketici veya başka bir rakip tüketici tarafından tekrar alınabilir kullanılabilir hale getirmek hizmet veri yolu sağlar. İkincisi, kilidi ile ilişkili bir zaman aşımı yoktur ve (örneğin, uygulama çökerse) Service Bus iletinin kilidini açar ve kolaylaştırır kilit zaman aşımı dolmadan önce iletiyi işlemek uygulama başarısız olursa kullanılabilir olması yeniden alınan) temelde gerçekleştiren bir [AbandonAsync](/dotnet/api/microsoft.azure.servicebus.queueclient.abandonasync) işlemi varsayılan olarak).

Uygulama ileti, ancak önce çökmesi durumunda, **CompleteAsync** isteği bildirilmeden, yeniden başlatıldığında ileti uygulamaya tekrar teslim. Bu işlem genellikle adlı *en az bir kez* işleme; diğer bir deyişle, her ileti en az bir kez işlenir. Ancak, belirli durumlarda aynı ileti yeniden teslim edilebilir. İlave bir mantık yinelemeleri algılamak için uygulamada Required senaryo yinelenen işlemeyi, kabul etmiyorsa, elde edilebilir temel [MessageID](/dotnet/api/microsoft.azure.servicebus.message.messageid) boyunca sabit kalır iletinin özelliği Teslim girişimleri. Bu özellik olarak bilinen *tam olarak bir kez* işleme.

## <a name="topics-and-subscriptions"></a>Konular ve abonelikler

Kuyruklar, her ileti tek bir tüketici tarafından işlenir aksine *konuları* ve *abonelikleri* iletişim, bir çok form sağladığınız bir *Yayımlama/abonelik* düzeni. Çok sayıda alıcı için ölçeklendirme için yararlı yayımlanan her ileti konuya kaydedilen her abonelik için kullanılabilir yapılır. İletiler konu başlığına gönderilen ve bir veya daha fazla ilişkili abonelik, bir abonelik başına temelinde ayarlanabilir filtre kuralları bağlı olarak teslim edilir. Abonelikler ek filtreler almak istediği iletileri kısıtlamak için kullanabilirsiniz. İletileri gönderilir aynı şekilde konu kuyruğa gönderilmeden ancak iletileri alınmayan konusundan doğrudan. Bunun yerine, bunlar aboneliklerden alınır. Bir konu aboneliği konu başlığına gönderilen iletilerin kopyalarını alan sanal kuyruğa benzer. İletileri bir abonelikten bir kuyruktan alınan şekilde aynı aldı.

Karşılaştırma, aracılığıyla doğrudan bir konuya bir kuyruk iletisi göndererek işlevselliğini eşler ve bir abonelik için ileti alma işlevselliğini eşler. Bunun yanı sıra, abonelikleri sıraları açısından bu bölümde daha önce açıklanan aynı düzenleri desteği bu özellik anlamına gelir: Rakip tüketici, zamana bağlı ayırma, Yük Dengeleme ve Yük Dengeleme.

### <a name="create-topics-and-subscriptions"></a>Konu başlıkları ve abonelikler oluşturun

Bir konu oluşturma önceki bölümde açıklandığı gibi bir kuyruk oluşturmak için benzerdir. Daha sonra kullanarak iletileri göndereceğiniz [TopicClient](/dotnet/api/microsoft.azure.servicebus.topicclient) sınıfı. İletileri almak için bir veya daha fazla abonelik konusuna oluşturun. Benzer şekilde sıralar, ileti kullanarak bir abonelik alındığında bir [SubscriptionClient](/dotnet/api/microsoft.azure.servicebus.subscriptionclient) yerine Nesne bir [QueueClient](/dotnet/api/microsoft.azure.servicebus.queueclient) nesnesi. Konu adı, abonelik ve (isteğe bağlı) alma modu adını parametre olarak geçirme abonelik istemci oluşturun. 

Bir tam için bkz: Örneğin, çalışma [BasicSendReceiveUsingTopicSubscriptionClient örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/BasicSendReceiveUsingTopicSubscriptionClient) github'da.

### <a name="rules-and-actions"></a>Kurallar ve eylemler

Birçok senaryoda belirli özelliklere sahip iletileri farklı şekillerde işlenmelidir. Bu işlemi etkinleştirmek için bu özellikler istenilen ve bu özellikler için bazı değişiklikleri gerçekleştirin iletileri bulmak için abonelikleri yapılandırabilirsiniz. Hizmet veri yolu abonelikleri konu başlığına gönderilen tüm iletiler görürsünüz, ancak bu iletiler bir kısmı yalnızca sanal abonelik kuyruğuna kopyalayabilirsiniz. Bu filtreleme, abonelik filtreleri kullanılarak gerçekleştirilir. Tür değişiklikler adlı *filtre eylemlerini*. Bir abonelik oluşturduğunuzda, Sistem özellikleri her iki ileti özellikleri üzerinde çalıştığı bir filtre ifadesi sağlayabilirsiniz (örneğin, **etiket**) ve özel uygulama özelliklerini (örneğin,  **StoreName**.) SQL filtre ifadesi bu durumda isteğe bağlıdır; bir SQL filtre ifadesi, bu abonelik için tüm iletiler bir abonelikte tanımlanan herhangi bir filtre işlem gerçekleştirilir.

Bir tam için bkz: Örneğin, çalışma [TopicSubscriptionWithRuleOperationsSample örnek](https://github.com/Azure/azure-service-bus/tree/master/samples/DotNet/GettingStarted/Microsoft.Azure.ServiceBus/TopicSubscriptionWithRuleOperationsSample) github'da.

Olası filtre değerleri hakkında daha fazla bilgi için belgelerine bakın [SqlFilter](/dotnet/api/microsoft.azure.servicebus.sqlfilter) ve [SqlRuleAction](/dotnet/api/microsoft.azure.servicebus.sqlruleaction) sınıfları. 

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi ve Service Bus Mesajlaşma kullanma örnekleri için aşağıdaki konuları Gelişmiş bakın:

* [Service Bus mesajlaşma hizmetine genel bakış](service-bus-messaging-overview.md)
* [Hızlı Başlangıç: Azure portal ve .NET kullanarak ileti gönderip](service-bus-quickstart-portal.md)
* [Öğretici: Azure portal ve konuları/abonelikleri kullanarak Güncelleştirmesi Envanter](service-bus-tutorial-topics-subscriptions-portal.md)


