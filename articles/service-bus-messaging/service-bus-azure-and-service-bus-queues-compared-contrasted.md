---
title: "Azure depolama kuyrukları ve Service Bus kuyruklarını karşılaştırıldığında ve contrasted | Microsoft Docs"
description: "İki tür Azure tarafından sunulan kuyruk arasındaki benzerlikler ve farkları analiz eder."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 11/08/2017
ms.author: sethm
ms.openlocfilehash: f13c7330c9e828abe6557149b9a31c7170e33dcd
ms.sourcegitcommit: cf42a5fc01e19c46d24b3206c09ba3b01348966f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/29/2017
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Depolama kuyrukları ve Service Bus kuyruklarını - karşılaştırılan ve contrasted
Bu makalede farklar ve iki tür bugün Microsoft Azure tarafından sunulan kuyruk arasındaki benzerlikler Çözümler: depolama kuyrukları ve Service Bus kuyruklarını. Bu bilgileri kullanarak, ilgili teknolojileri karşılaştırabilir ve gereksinimlerinize en uygun çözümü seçerken daha bilinçli kararlar verebilirsiniz.

## <a name="introduction"></a>Giriş
Azure kuyruk mekanizmasıyla iki türlerini destekler: **depolama kuyrukları** ve **Service Bus kuyruklarını**.

**Depolama kuyrukları**, parçası olduğu [Azure depolama](https://azure.microsoft.com/services/storage/) altyapı, özellik içinde ve hizmetleri arasında güvenilir ve kalıcı Mesajlaşma sağlayan basit bir GET/PUT/GÖZLEM REST tabanlı arabirim.

**Service Bus kuyruklarını** daha geniş bir parçası olan [Azure Mesajlaşma](https://azure.microsoft.com/services/service-bus/) altyapısını destekleyen Yayınla/Abone ol yanı sıra queuing ve daha gelişmiş tümleştirme desenleri. Service Bus kuyrukları/konuları/abonelikler hakkında daha fazla bilgi için bkz: [Service Bus genel bakış](service-bus-messaging-overview.md).

Depolama kuyrukları queuing iki teknolojiyi aynı anda mevcut olsa da, ilk Azure Storage hizmetleri üzerine inşa ayrılmış kuyruk depolama mekanizması olarak eklenmiştir. Service Bus kuyrukları, uygulamaları veya birden çok iletişim protokolü, veri sözleşmeleri, güven etki alanları ve/veya ağ ortamları yayılabilir uygulama bileşenlerini tümleştirmek üzere tasarlanmış daha geniş Mesajlaşma altyapısının üzerinde oluşturulmuştur.

## <a name="technology-selection-considerations"></a>Teknoloji seçimi konuları
Depolama kuyrukları ve Service Bus kuyruklarını message queuing hizmeti şu anda Microsoft Azure tarafından sunulan uygulamalarıdır. Her ikisinden birini seçin veya her ikisi de belirli çözüm veya iş/teknik sorun çözme gereksinimlerine bağlı olarak kullanmak anlamına gelir biraz farklı özellik kümesi vardır.

Belirli bir çözüm amaçla hangi queuing teknolojisi uygun belirlerken, çözüm mimarları ve geliştiricileri bu önerileri göz önünde bulundurmalısınız. Daha fazla ayrıntı için sonraki bölüme bakın.

Bir çözümü Mimarı/geliştirici, olarak **depolama kuyruklarını kullanmayı düşünmelisiniz** zaman:

* Uygulamanızın üzerinde 80 GB iletileri iletileri 7 günden daha kısa bir ömre sahip olduğu bir kuyrukta depolamanız gerekir.
* Uygulamanızı bir ileti sırası içinde işlemek için ilerleme durumunu izlemek istiyor. Bu ileti işlenirken çalışan çökmesi durumunda faydalı olur. Bir sonraki alt sonra önceki çalışan burada bıraktığınız gelen devam etmek için bu bilgileri kullanabilirsiniz.
* Sunucu tarafı günlükleri tüm, kuyruklar karşı yürütülen işlemlerin gerektirir.

Bir çözümü Mimarı/geliştirici, olarak **Service Bus kuyruklarını kullanmayı düşünmelisiniz** zaman:

* Çözümünüzü sıranın sorgulamak zorunda kalmadan iletilerini kurabilmesi gerekir. Service Bus ile bu kullanımı ile uzun yoklama elde edilebilir Service Bus destekleyen TCP dayanan protokoller kullanarak işlemini alırsınız.
* Sıranın bir garantili ilk-giren ilk çıkar sağlamak için çözümünüzün gerektirir (FIFO) sıralı teslim.
* Simetrik bir deneyim Azure ve Windows Server (özel bulut) üzerinde istediğiniz. Daha fazla bilgi için bkz: [Windows Server için hizmet veri yolu](https://msdn.microsoft.com/library/dn282144.aspx).
* Çözümünüzü otomatik yinelenen algılama destekleyebilmesi gerekir.
* İşlem iletilerinin uygulamanıza paralel uzun süre çalışan akış olarak istediğiniz (ileti akışı kullanarak ilişkili [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid) ileti özelliği). Bu modelde, kullanıcı uygulama her düğüme ileti aksine akışlar için rekabet. Bir akış süren bir düğüme verildiğinde düğüm işlemleri kullanarak uygulama akışı durumunu durumunu inceleyebilirsiniz.
* Çözümünüzü işlem davranışı ve gönderme ya da birden fazla ileti kuyruktan alırken kararlılık gerektirir.
* Yaşam süresi (TTL) karakteristiğini uygulamaya özgü iş yükü, 7 günlük sürede aşabilir.
* Uygulamanız 64 KB aşabilir iletilerini işleme ancak olası değil yaklaşım 256 KB sınırlar.
* Göndericiler ile alıcılar için bir rol tabanlı erişim modeli kuyruklara ve farklı rights/izinler sağlamak için bir gereksinim ile ilgilidir.
* Sıra boyutu 80 GB'den büyük büyüyecektir değil.
* AMQP 1.0 standartlara dayalı Mesajlaşma protokolünü kullanmak istediğiniz. AMQP hakkında daha fazla bilgi için bkz: [hizmet veri yolu AMQP genel bakış](service-bus-amqp-overview.md).
* Bir son geçiş sırası tabanlı noktadan noktaya iletişim sorunsuz tümleştirilmesi ek alıcılar (aboneleri için), her biri bazı veya tüm bağımsız bir kopyasını alır sağlayan bir ileti değişim deseni planladığınız sıraya gönderilen iletileri. İkinci yerel olarak Service Bus tarafından sağlanan Yayımla ve abone özellik başvuruyor.
* Mesajlaşma çözümü ek altyapı bileşenleri oluşturmanıza gerek kalmadan "En-çoğu-kez" teslim garantisi destekleyebilmesi gerekir.
* Yayımlama ve iletileri toplu kullanmak ister misiniz?

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Depolama kuyrukları ve Service Bus kuyruklarını karşılaştırma
Aşağıdaki bölümlerdeki tablolar sıra özellikleri mantıksal bir gruplandırmasını sağlar ve, hem Azure Storage sıraları hem de hizmet veri yolu sıraları kullanılabilen özellikleri bir bakışta karşılaştırmanıza olanak tanır.

## <a name="foundational-capabilities"></a>Temel özellikler
Bu bölümde bazı depolama kuyrukları ve Service Bus kuyrukları ile sağlanan temel queuing özelliklerini karşılaştırır.

| Karşılaştırma ölçütü | Depolama kuyrukları | Service Bus Kuyrukları |
| --- | --- | --- |
| Garanti sıralama |**Hayır** <br/><br>Daha fazla bilgi için listedeki ilk nota "Ek bilgiler" bölümüne bakın.</br> |**Evet - ilk-giren ilk çıkar (FIFO)**<br/><br>(oturumları Mesajlaşma kullanımı ile) |
| Teslim garantisi |**En az bir kere** |**En az bir kere**<br/><br/>**En çok-bir kez** |
| Atomik işlem desteği |**Hayır** |**Evet**<br/><br/> |
| Davranış alma |**Olmayan engelleme**<br/><br/>(hemen yeni bir ileti bulunursa tamamlandıktan) |**Zaman aşımı olan ve olmayan engelleme**<br/><br/>(uzun yoklama sunar veya ["Comet teknik"](http://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Olmayan engelleme**<br/><br/>(kullanımı ile .NET API yalnızca yönetilen) |
| Anında iletme stili API |**Hayır** |**Evet**<br/><br/>[Onmessageoptions](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) ve **Onmessageoptions** oturumları .NET API. |
| Mod alma |**Peek & kira** |**Peek & Kilitle**<br/><br/>**Alma & Sil** |
| Özel erişim modu |**Kira tabanlı** |**Kilit tabanlı** |
| Kira/süre kilidi |**30 saniye (varsayılan)**<br/><br/>**7 gün (en)** (yenileme veya iletiyi kullanarak kira serbest [UpdateMessage](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage) API.) |**60 saniye (varsayılan)**<br/><br/>İletiyi kullanarak kilit yenileyebilirsiniz [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API. |
| Kira/duyarlık kilidi |**İleti düzeyi**<br/><br/>(her ileti sonra gereken şekilde kullanarak ileti işlenirken güncelleştirebilirsiniz bir başka zaman aşımı değerine sahip [UpdateMessage](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage) API) |**Sıranın düzeyi**<br/><br/>(tüm iletileri uygulanmış bir kilit duyarlık her sıranın vardır, ancak Kilitle kullanarak yenileyebilirsiniz [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API.) |
| Toplu alma |**Evet**<br/><br/>(açıkça ileti sayısı en çok 32 iletileri iletileri alınırken belirtme) |**Evet**<br/><br/>(örtük olarak getirme öncesi özellik etkinleştirme veya açıkça işlemleri kullanarak) |
| Toplu iş gönderme |**Hayır** |**Evet**<br/><br/>(işlemler veya istemci tarafı toplu işleme kullanımı ile) |

### <a name="additional-information"></a>Ek bilgiler
* Depolama sıralarındaki iletileri genellikle ilk-giren ilk çıkar ancak bazen bozuk olabilirler; Örneğin, bir iletinin görünürlük zaman aşımı süresi (örneğin, bir istemci uygulaması işleme sırasında kilitlenen) sonucunda süresi dolduğunda. Görünürlük zaman aşımı süresi dolduğunda, ileti yeniden sıranın dequeue başka bir çalışan için görünür olur. Bu noktada, yeni görünür ileti sıraya (yeniden kuyruktan çıkarıldı için), ilk olarak sıraya alınan bundan sonra bir ileti sonra yerleştirilmiş olabilir.
* Service Bus kuyruklarını garantili FIFO desende Mesajlaşma oturumları kullanılmasını gerektirir. Alınan ileti işlenirken bir uygulama kilitleniyor gerektiğinde, **Peek & Kilitle** modu, bir sıra alıcı kabul bir Mesajlaşma oturumu açtığında, başlayacak başarısız iletisiyle, yaşam süresi (TTL sonra) süresi sona eriyor.
* Uygulama bileşenleri kesilmesi ölçeklenebilirlik ve dayanıklılık hatalar için artırmak için Yük Dengeleme ve süreç iş akışlarının oluşturulmasını gibi depolama kuyrukları standart queuing senaryoları desteklemek için tasarlanmıştır.
* Service Bus kuyrukları Destek *en az bir kere* teslim garanti edilemez. Ayrıca, *adresindeki çoğu-kez* anlamsal uygulama durumunu depolamak için oturum durumu kullanmanın ve otomatik olarak ileti alma ve oturum durumu güncelleştirmek için işlemleri kullanarak desteklenebilir.
* Geliştiriciler hem işletim ekipleri, kuyruklar, tablolar ve BLOB'lar – arasında Tekdüzen ve tutarlı bir programlama modeli depolama kuyrukları sağlar.
* Service Bus kuyruklarını yerel hareketlerinin tek bir sıraya için bağlamında destek sağlar.
* **Alma ve silme** Service Bus tarafından desteklenen modu alçaltılmış teslim güvence karşılığında Mesajlaşma işlem sayısını (ve ilişkili maliyet) azaltma olanağı sağlar.
* Depolama kuyrukları kiraları iletileri için kira sürelerini genişletme olanağı sağlar. Bu iletileri kısa kira korumak çalışanları sağlar. Bir çalışan kilitlenirse bu durum, bu nedenle, ileti hızlı bir şekilde yeniden başka bir çalışan tarafından işlenebilir. Ayrıca, bu işlem geçerli kiralama süresinden daha uzun gerektiriyorsa bir çalışan bir ileti kira genişletebilirsiniz.
* Depolama kuyrukları nedeniyle ayarlayın veya bir ileti kuyruktan alma yapabileceğiniz bir görünürlük zaman aşımını sunar. Ayrıca, çalışma zamanında farklı kira değerlerle bir ileti güncelleştirin ve aynı sıradaki iletiler arasında farklı değerleri güncelleştirin. Hizmet veri yolu kilit zaman aşımı sıra meta verilerde tanımlanan; çağırarak kilidi ancak yenileyebilirsiniz [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) yöntemi.
* Service Bus kuyruklarını işlemi bir engelleme almak için en uzun zaman aşımı 24 gündür. Ancak, REST tabanlı zaman aşımları 55 saniyelik en fazla bir değere sahip.
* Service Bus tarafından sağlanan istemci-tarafı toplu işleme sırası tek gönderme işlemi birden fazla ileti toplu istemciye sağlar. Toplu işleme yalnızca zaman uyumsuz gönderme işlemleri için kullanılabilir.
* (Daha fazla hesapları sanallaştırmak olduğunda) depolama kuyrukları ve sınırsız sıralarını 200 TB'ye tavan gibi özellikler SaaS sağlayıcıları için ideal bir platform kolaylaştırır.
* Kullanıcı erişim denetimi mekanizması temsilci ve depolama sorguları bir esnek sağlar.

## <a name="advanced-capabilities"></a>Gelişmiş Özellikler
Bu bölüm, depolama kuyrukları ve Service Bus kuyruklarını tarafından sağlanan gelişmiş özellikleri karşılaştırır.

| Karşılaştırma ölçütü | Depolama kuyrukları | Service Bus Kuyrukları |
| --- | --- | --- |
| Zamanlanan teslim |**Evet** |**Evet** |
| Otomatik ölü harflerinin |**Hayır** |**Evet** |
| Artan sıra yaşam süresi değeri |**Evet**<br/><br/>(yerinde görünürlük zaman aşımını güncelleştirilmesini) |**Evet**<br/><br/>(ayrılmış bir API işlevi sağlanır) |
| Zehirli ileti desteği |**Evet** |**Evet** |
| Yerinde güncelleştirme |**Evet** |**Evet** |
| Sunucu tarafında işlem günlüğü |**Evet** |**Hayır** |
| Depolama ölçümleri |**Evet**<br/><br/>**Ölçümleri dakika**: kullanılabilirlik, TP'leri, API için gerçek zamanlı ölçümleri çağrı sayısı, hata sayısı ve tüm gerçek (dakika başına toplanır ve yalnızca üretimde ne gelen birkaç dakika içinde bildirilen. zaman içinde daha sağlar Daha fazla bilgi için bkz: [Storage Analytics ölçümleri hakkında](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Evet**<br/><br/>(Toplu sorguları çağırarak [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Durum Yönetimi |**Hayır** |**Evet**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus.active), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.disabled), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.senddisabled), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus.receivedisabled) |
| Otomatik iletme iletisi |**Hayır** |**Evet** |
| Sıra işlevi Temizle |**Evet** |**Hayır** |
| İleti grupları |**Hayır** |**Evet**<br/><br/>(oturumları Mesajlaşma kullanımı ile) |
| İleti grubu başına uygulama durumu |**Hayır** |**Evet** |
| Yinelenen algılama |**Hayır** |**Evet**<br/><br/>(Gönderen tarafında yapılandırılabilir) |
| Gözatma ileti grupları |**Hayır** |**Evet** |
| İleti oturumları Kimliğine göre getirme |**Hayır** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Sıraya alma iki teknolojiyi bir ileti teslimi için daha sonraki bir zamanda zamanlanacak etkinleştirin.
* Sıra otomatik iletme otomatik iletme iletilerinin içinden alma işlemini yapan uygulamanın iletiyi tüketir tek bir sıraya için kuyrukları binlerce sağlar. Güvenliği sağlamak, akışını denetlemek için bu düzenek kullanın ve her ileti yayımcı arasında depolama yalıtır.
* Depolama kuyrukları, ileti içeriği güncelleştirmek için destek sağlar. Böylece sıfırdan yerine en son bilinen kontrol noktasından işlenebilir, bu işlev kalıcı durum bilgilerini ve artımlı ilerleme durumu güncelleştirmeleri için iletiye kullanabilirsiniz. Service Bus kuyrukları ile aynı senaryo ileti oturumları kullanarak etkinleştirebilirsiniz. Oturumları etkinleştir, kaydetme ve uygulama işleme durumunu almak (kullanarak [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) ve [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Kullanılmayan lettering](service-bus-dead-letter-queues.md), olduğu yalnızca Service Bus kuyruklarını tarafından desteklenir, işlenemiyor başarıyla alıcı uygulama tarafından veya ne zaman iletileri erişemiyor hedeflerine süresi dolmuş bir yaşam süresi (nedeniyle iletileri yalıtmak için kullanışlı olabilir TTL) özelliği. TTL değeri ne kadar bir ileti kuyruğunda kalır belirtir. Service Bus ile TTL süresi sona erdiğinde $DeadLetterQueue adlı özel bir kuyruğuna ileti taşınır.
* "Zarar" iletileri depolama kuyruklarda bir iletiyi kuyruktan alma uygulama bulma incelediği [DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx) iletinin özelliği. Varsa **DequeueCount** belirli bir eşik değerinden yüksek uygulama bir uygulama tarafından tanımlanan "sahipsiz" kuyruğuna ileti taşır.
* Depolama kuyrukları tüm sırası iyi olarak toplanan ölçümler olarak yürütülen işlemlerin ayrıntılı günlüğü almak etkinleştirin. Bu seçeneklerin ikisi de hata ayıklama ve depolama sorguları uygulamanızı nasıl kullandığını anlamak için kullanışlıdır. Ayrıca, uygulamanızın performans ayarlama ve kuyrukları kullanma maliyetlerini azaltmak için yararlıdır.
* "Service Bus tarafından desteklenen ileti oturumları" kavramı sırayla iletileri ve bunların ilgili alıcılar arasında oturum benzeri benzeşim oluşturur belirli bir alıcı ilişkilendirilecek bir belirli mantıksal gruba ait iletileri sağlar. Bu hizmet veri yolu işlevindeki ayarlayarak Gelişmiş etkinleştirebilirsiniz [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) bir ileti özelliği. Alıcıları belirli bir oturum kimliği üzerinde dinleme ve belirtilen oturum tanımlayıcısını paylaşan iletileri alacak.
* Service Bus kuyruklarını tarafından otomatik olarak desteklenen yineleme algılama işlevi bir kuyruk veya konu değerine göre gönderilen yinelenen iletileri kaldırır [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) özelliği.

## <a name="capacity-and-quotas"></a>Kapasite ve kotaları
Bu bölümde depolama kuyrukları ve Service Bus kuyruklarını açısından karşılaştırır [kapasiteyi ve kotayı](service-bus-quotas.md) geçerli olabilir.

| Karşılaştırma ölçütü | Depolama kuyrukları | Service Bus Kuyrukları |
| --- | --- | --- |
| En büyük sıra boyutu |**500 TB**<br/><br/>(sınırlı bir [tek bir depolama hesabı kapasitesi](../storage/common/storage-introduction.md#queue-storage)) |**80 GB için 1 GB**<br/><br/>(kuyruk oluşturma sırasında tanımlanan ve [bölümleme etkinleştirme](service-bus-partitioning.md) – "Ek bilgiler" bölümüne bakın) |
| İleti boyutu üst sınırı |**64 KB**<br/><br/>(48 kullanırken KB **Base64** kodlama)<br/><br/>Azure, kuyruklar ve BLOB'lar – bu noktada, şunları yapabilirsiniz enqueue birleştirerek büyük iletileri destekleyen tek bir öğe için en fazla 200 GB. |**256 KB** veya **1 MB**<br/><br/>(başlık ve gövde, en fazla üstbilgi boyutu dahil: 64 KB).<br/><br/>Bağımlı [hizmet katmanı](service-bus-premium-messaging.md). |
| En fazla ileti TTL |**7 gün** |**TimeSpan.Max** |
| Kuyruğu en yüksek sayısı |**Sınırsız** |**10,000**<br/><br/>(hizmet ad alanı) |
| Maksimum eşzamanlı istemci sayısı |**Sınırsız** |**Sınırsız**<br/><br/>(yalnızca 100 eş zamanlı bağlantı sınırı TCP protokolü tabanlı iletişim'geçerlidir) |

### <a name="additional-information"></a>Ek bilgiler
* Hizmet veri yolu kuyruğu boyutu sınırları zorlar. En büyük sıra boyutu sıra oluşturma sırasında belirtilir ve 1 ile 80 GB arasında bir değer olabilir. Sıra oluşturma ayarlanan sıra boyutu değeri ulaştıysanız, ek gelen iletileri reddedilir ve bir özel durum çağrıyı yapan kod tarafından alınır. Service Bus kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları](service-bus-quotas.md).
* İçinde [standart katmanı](service-bus-premium-messaging.md), Service Bus kuyruklarını boyutlarında, 1, 2, 3, 4 veya 5 GB (varsayılan olarak 1 GB) oluşturabilirsiniz. Premium katmanındaki kuyrukları oluşturabilirsiniz 80 GB boyutunda. Standart bölümlendirme ile katmanı, etkin (varsayılan olmayan), hizmet veri yolu için belirttiğiniz her GB 16 bölümler oluşturur. 5 GB cinsinden boyutu olan bir kuyruk oluşturun, bu nedenle, 16 bölümlerle en büyük sıra boyutu (5 * 16) haline gelir = 80 GB. Kendi girdisi bakarak bölümlenmiş kuyruk veya konu en büyük boyutunu görebilirsiniz [Azure portal][Azure portal]. Premium katmanındaki yalnızca 2 bölüm sıra oluşturulur.
* Depolama kuyruklarla iletinin içeriğini XML uyumlu değilse, bu olmalıdır **Base64** kodlanmış. Varsa, **Base64**-ileti kodlama, kullanıcı yükü 64 KB yerine kadar 48 KB olabilir.
* Service Bus kuyrukları ile bir sırada depolanan her ileti iki bölümden oluşur: bir başlık ve gövde. İleti toplam boyutu hizmet katmanı tarafından desteklenen maksimum ileti boyutu aşamaz.
* İstemciler ile Service Bus kuyrukları TCP protokolü üzerinden iletişim kurduğunda, tek bir Service Bus kuyruğuna eşzamanlı bağlantı sayısının 100 sınırlıdır. Bu numara göndericiler ile alıcılar arasında paylaşılır. Bu kotasına ulaşıldığında, sonraki istekleri için ek bağlantıları reddedilir ve bir özel durum çağrıyı yapan kod tarafından alınır. Bu sınır, REST tabanlı API kullanarak kuyruklarına bağlanan istemcilerde uygulanan değil.
* Tek bir hizmet veri yolu ad alanı 10. 000'den fazla kuyruklarda ihtiyacınız varsa Azure Destek ekibine başvurun ve artışı isteyebilir. Hizmet veri yolu 10.000 kuyruklarla ötesinde ölçeklendirme için de ek ad alanlarını kullanarak oluşturabilirsiniz [Azure portal][Azure portal].

## <a name="management-and-operations"></a>Yönetim ve işlemler
Bu bölüm, depolama kuyrukları ve Service Bus kuyruklarını tarafından sağlanan yönetim özellikleri karşılaştırılır.

| Karşılaştırma ölçütü | Depolama kuyrukları | Hizmet Veri Yolu kuyrukları |
| --- | --- | --- |
| Yönetim Protokolü |**HTTP/HTTPS üzerinden getirin** |**HTTPS üzerinden getirin** |
| Çalışma zamanı Protokolü |**HTTP/HTTPS üzerinden getirin** |**HTTPS üzerinden getirin**<br/><br/>**AMQP 1.0 Standart (TCP TLS ile)** |
| .NET API’si |**Evet**<br/><br/>(.NET depolama İstemcisi API) |**Evet**<br/><br/>(.NET Service Bus API) |
| Yerel C++ |**Evet** |**Evet** |
| Java API’si |**Evet** |**Evet** |
| PHP API |**Evet** |**Evet** |
| Node.js API |**Evet** |**Evet** |
| İsteğe bağlı meta veri desteği |**Evet** |**Hayır** |
| Sıra adlandırma kuralları |**En fazla 63 karakter uzunluğunda**<br/><br/>(Bir sıra adı harfler küçük olmalıdır.) |**En fazla 260 karakter uzunluğunda**<br/><br/>(Kuyruk yollar ve adları büyük küçük harfe duyarsızdır.) |
| Get kuyruk uzunluğu işlevi |**Evet**<br/><br/>(Silinmiş olmadan iletileri TTL zaman aşımına uğrarsa yaklaşık değeri.) |**Evet**<br/><br/>(Değer. tam, zaman içinde nokta) |
| Peek işlevi |**Evet** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Depolama kuyrukları, ad/değer çiftleri biçiminde sıra açıklama uygulanabilir rasgele öznitelikleri için destek sağlar.
* Her iki sıra teknolojileri queue explorer/tarayıcı aracı uygularken yararlı olabilen kilitleyebilirsiniz zorunda kalmadan bir iletiye olanağı sunar.
* Hizmet veri yolu .NET Mesajlaşma API'lerini Dengeleme tam çift yönlü TCP bağlantıları için REST HTTP üzerinden karşılaştırıldığında performansı için aracılı ve AMQP 1.0 standart protokolünü destekler.
* Depolama sıraların adları 3-63 karakter uzunluğunda olabilir, küçük harf, sayı ve tire içerebilir. Daha fazla bilgi için bkz: [adlandırma kuyrukları ve meta verileri](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* Hizmet veri yolu kuyruğu adları en fazla 260 karakter uzunluğunda olmalıdır ve daha az kısıtlayıcı adlandırma kurallarına sahip. Hizmet veri yolu kuyruğu adlar harf, rakam, nokta, kısa çizgi ve alt çizgi içerebilir.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Bu bölümde depolama kuyrukları ve Service Bus kuyruklarını tarafından desteklenen kimlik doğrulama ve yetkilendirme özellikleri açıklanmaktadır.

| Karşılaştırma ölçütü | Depolama kuyrukları | Service Bus Kuyrukları |
| --- | --- | --- |
| Kimlik Doğrulaması |**Simetrik anahtar** |**Simetrik anahtar** |
| Güvenlik modeli |SAS belirteci üzerinden yetkilendirilmiş erişim. |SAS |
| Kimlik sağlayıcısı Federasyon |**Hayır** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Sıraya alma teknolojileri ya da her isteğin kimliğinin doğrulanması gerekir. Anonim erişimi olan genel sıralar desteklenmiyor. Kullanarak [SAS](service-bus-sas.md), bu senaryo yalnızca yazma SAS, salt okunur SAS veya tam erişim SAS yayımlayarak karşılayabilir.
* Kimlik doğrulama şeması tarafından depolama sıraları içerir sağlanan bir karma tabanlı ileti kimlik doğrulama kodu (HMAC) bir simetrik anahtar kullanımını SHA-256 algoritması ile hesaplanır ve olarak kodlanmış bir **Base64** dize. İlgili protokolü hakkında daha fazla bilgi için bkz: [Azure Storage Hizmetleri için kimlik doğrulaması](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Service Bus kuyruklarını simetrik anahtarlar kullanılarak benzer bir modelini destekler. Daha fazla bilgi için bkz: [paylaşılan erişim imzası kimlik doğrulaması Service Bus ile](service-bus-sas.md).

## <a name="conclusion"></a>Sonuç
İki teknolojiyi daha derin bir anlayış kazandıktan tarafından size kullanmak için hangi sıra teknolojisine daha bilinçli bir karar kuramaz ve ne zaman. Depolama sıralar veya Service Bus kuyruklarını kullanma zamanı kararını açıkça bir dizi etkene bağlıdır. Bu etkenler yoğun ihtiyaçlara uygulamanız ve mimarisinin bağlı olabilir. Uygulama zaten Microsoft Azure temel özelliklerini kullanıyorsa, özellikle temel iletişimi ve Hizmetleri veya 80 GB boyutunda daha büyük olabilir gerek sıraları arasında Mesajlaşma gerekiyorsa, depolama kuyruklarında seçmek tercih edebilirsiniz.

Service Bus kuyruklarını birkaç oturumları, işlemleri gibi gelişmiş özellikler sağlamak için yinelenen saptama, otomatik ulaşmayan posta ve sürekli yayımlama/abonelik özellikleri, bunlar olabilir bir tercih edilen seçenek karma oluşturuluyorsa uygulama veya aksi durumda, uygulamanızın bu özelliklerden gerektirip gerektirmediğini.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makalelerde, daha fazla yönerge ve depolama sorguları ya da Service Bus kuyruklarını kullanma hakkında bilgi sağlanır.

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Kuyruk depolama hizmetini kullanma](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Aracılı Mesajlaşma kullanan Service Bus performans iyileştirmeleri için en iyi yöntemler](service-bus-performance-improvements.md)
* [Kuyruklar ve konu başlıkları Azure Service Bus (blog yayını) içinde Tanıtımı](http://www.code-magazine.com/article.aspx?quickid=1112041)
* [Hizmet veri yolu için Geliştirici Kılavuzu](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Sıraya alma hizmetidir Azure'da kullanma](http://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

