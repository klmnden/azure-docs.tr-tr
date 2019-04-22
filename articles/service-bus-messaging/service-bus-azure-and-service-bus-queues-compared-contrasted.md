---
title: Azure depolama kuyrukları ve Service Bus kuyrukları benzerlikler ve karşıtlıklar | Microsoft Docs
description: Farklılıklar ve Azure tarafından sağlanan iki tür arasındaki benzerlikler analiz eder.
services: service-bus-messaging
documentationcenter: na
author: axisc
manager: timlt
editor: spelluru
ms.assetid: f07301dc-ca9b-465c-bd5b-a0f99bab606b
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 01/23/2019
ms.author: aschhab
ms.openlocfilehash: 2086813b01de6cd06f3714477e56864b36196382
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699056"
---
# <a name="storage-queues-and-service-bus-queues---compared-and-contrasted"></a>Depolama kuyrukları ve Service Bus kuyrukları - benzerlikler ve karşıtlıklar
Bu makalede, iki tür kuyruk bugün Microsoft Azure tarafından sunulan arasındaki benzerlikleri ve farkları analiz eder: Depolama kuyrukları ve Service Bus kuyrukları. Bu bilgileri kullanarak, ilgili teknolojileri karşılaştırabilir ve gereksinimlerinize en uygun çözümü seçerken daha bilinçli kararlar verebilirsiniz.

## <a name="introduction"></a>Giriş
Azure, sıra mekanizmalar iki türlerini destekler: **Depolama kuyrukları** ve **Service Bus kuyruklarını**.

**Depolama kuyrukları**, parçası olduğu [Azure depolama](https://azure.microsoft.com/services/storage/) altyapısı özelliği içinde ve hizmetler arasında güvenilir ve kalıcı Mesajlaşma sağlayan basit bir GET/PUT/GÖZLEM REST tabanlı arabirim.

**Service Bus kuyruklarını** daha geniş bir parçası olan [Azure Mesajlaşma](https://azure.microsoft.com/services/service-bus/) destekleyen yayımlama/abone olma yanı sıra sıraya alma altyapısı ve daha gelişmiş tümleştirme desenleri. Service Bus kuyrukları/konular/abonelikler hakkında daha fazla bilgi için bkz: [Service Bus bakış](service-bus-messaging-overview.md).

Depolama kuyrukları, sıraya alma iki teknolojiyi aynı anda mevcut olsa da, ilk olarak, Azure depolama hizmetleri üzerinde oluşturulmuş bir adanmış kuyruk depolama mekanizması olarak eklenmiştir. Service Bus kuyrukları, uygulamaları veya birden fazla iletişim protokolleri, veri sözleşmeleri, güven etki alanları ve/veya ağ ortamları kapsayabilir uygulama bileşenlerini tümleştirmek için tasarlanan daha geniş Mesajlaşma altyapısı üzerine oluşturulur.

## <a name="technology-selection-considerations"></a>Teknoloji seçimi konuları
Hem depolama kuyrukları ve Service Bus kuyrukları, şu anda Microsoft Azure tarafından sunulan hizmet ileti uygulamalarıdır. Her birini veya diğerini seçin veya her ikisi de belirli bir çözüm ya da iş/teknik sorun çözme gereksinimlerine bağlı olarak kullanmak anlamına gelir. bir biraz farklı özellik kümesine sahiptir.

Sıraya alma teknolojileri belirli bir çözüm için amaca uygun belirlerken, çözüm mimarları ve geliştiricileri bu önerileri düşünmelisiniz. Diğer ayrıntılar için sonraki bölüme bakın.

/ Geliştirici, çözüm Mimarı olarak **depolama kuyruklarını kullanmayı düşünmeniz gerekir** olduğunda:

* Uygulamanızı bir kuyrukta ileti üzerinde 80 GB depolamanız gerekir.
* Uygulamanızın içinde kuyruğa bir ileti işleme için ilerleme durumunu izlemek istiyor. Bu ileti işlenirken çalışanın kilitlenmesi durumunda kullanışlıdır. Bir sonraki alt ardından burada önceki çalışan kaldığı gelen devam etmek için bu bilgileri kullanabilirsiniz.
* Sunucu tarafı günlüklerini tüm Kuyruklarınızı karşı yürütülen işlemlerin gerektirir.

/ Geliştirici, çözüm Mimarı olarak **Service Bus kuyruklarını kullanmayı düşünmeniz gerekir** olduğunda:

* Çözümünüzü sıra yoklamak gerek kalmadan iletileri almak mümkün olması gerekir. Service Bus ile bu kullanımı uzun yoklama ulaşılabilecek alma, Service Bus'ı destekleyen TCP tabanlı protokolleri kullanarak işlemi.
* Sıranın bir garantili ilk-giren ilk çıkar sağlamak için çözümünüzün gerektirir (FIFO) sıralı teslim.
* Çözümünüzü otomatik yinelenen algılama destekleyebilen olması gerekir.
* Uzun süre çalışan akışlar paralel olarak iletilerini işlemek için uygulamanızın istediğiniz (ileti kullanarak bir akış ilişkili [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid) ileti özelliği). Bu modelde, kullanıcı uygulamanın her bir düğümünde iletileri aksine, akışları için rekabet. Bir akışı kullanan bir düğüme verildiğinde düğüm işlemleri kullanarak uygulama akış durumu durumunu inceleyebilirsiniz.
* Çözümünüzü işlem davranışı ve gönderme veya kuyruktan birden çok ileti alırken kararlılık gerektirir.
* Uygulamanızı 64 KB aşabilir iletileri işler, ancak olası değil yaklaşım 256 KB sınırlar.
* Kuyruklar ve farklı rights/izinleri rol tabanlı erişim modeline göndericiler ve alıcılar için sağlamak için bir gereksinim ile ilgilidir.
* Kuyruk boyutu 80 GB'nin büyütün değil.
* AMQP 1.0 standartlara dayalı Mesajlaşma protokolü kullanmak istiyorsunuz. AMQP hakkında daha fazla bilgi için bkz: [hizmet veri yolu AMQP genel bakış](service-bus-amqp-overview.md).
* Kuyruk tabanlı noktadan noktaya iletişim nihai bir geçiş için ek alıcılar (aboneleri), her biri bazı veya tüm bağımsız bir kopyasını alır sorunsuz bir şekilde tümleştirilmesini sağlar bir ileti değişim deseni düşünebilirsiniz. kuyruğa gönderilen iletileri. İkinci yerel olarak hizmet veri yolu tarafından sağlanan yayımlama/abone olma yeteneği anlamına gelir.
* Mesajlaşma çözümünüzü, "En büyük bir kez" teslimi garanti ek altyapı bileşenleri oluşturmanıza gerek kalmadan destekleyebilen olmalıdır.
* Yayımlama ve iletileri toplu kullanmak ister misiniz?

## <a name="comparing-storage-queues-and-service-bus-queues"></a>Depolama kuyrukları ve Service Bus kuyruklarını karşılaştırması
Aşağıdaki bölümlerde tablolar kuyruk özellikleri mantıksal bir gruplandırmasını sağlar ve Azure depolama kuyrukları ve Service Bus kuyruklarını bulunan özelliklerin bir bakışta karşılaştırmanıza olanak tanır.

## <a name="foundational-capabilities"></a>Temel özellikleri
Bu bölümde depolama kuyrukları ve Service Bus kuyrukları ile sağlanan temel sıraya alma özelliklerden bazılarını karşılaştırılmıştır.

| Karşılaştırma ölçütleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Sıralama garantisi |**Hayır** <br/><br>Daha fazla bilgi için ilk Not "Ek bilgileri" bölümüne bakın.</br> |**Evet - ilk-giren ilk çıkar (FIFO)**<br/><br>(ileti oturumları kullanarak) |
| Teslimi garanti |**En az bir kez** |**En az bir kez**<br/><br/>**En büyük bir kez** |
| Atomik işlem desteği |**Hayır** |**Evet**<br/><br/> |
| Davranış alırsınız |**Engelleyici olmayan**<br/><br/>(yeni ileti bulunursa hemen tamamlanır) |**Zaman aşımı olan/olmayan engelleme**<br/><br/>(uzun yoklama sunar veya ["Comet teknik"](https://go.microsoft.com/fwlink/?LinkId=613759))<br/><br/>**Engelleyici olmayan**<br/><br/>(.NET kullanarak API sadece yönetilen) |
| Anında iletme stili API |**Hayır** |**Evet**<br/><br/>[Pushhandlerservice](/dotnet/api/microsoft.servicebus.messaging.queueclient.onmessage#Microsoft_ServiceBus_Messaging_QueueClient_OnMessage_System_Action_Microsoft_ServiceBus_Messaging_BrokeredMessage__) ve **Onmessageoptions** oturumlarının .NET API. |
| Modu alır |**Özet & kiralama** |**Özet & Kilitle**<br/><br/>**Alma ve silme** |
| Özel erişim modu |**Kira tabanlı** |**Kilit tabanlı** |
| Kira/kilit süresi |**30 saniye (varsayılan)**<br/><br/>**7 gün (maksimum)** (yenileyin veya ileti kullanarak kira serbest [UpdateMessage](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage) API.) |**60 saniye (varsayılan)**<br/><br/>Kullanarak bir ileti kilidi yenileme [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API. |
| Duyarlık kira/kilitleme |**İleti düzeyi**<br/><br/>(her ileti, ardından gerektiği kullanarak ileti işlenirken güncelleştirebilirsiniz bir farklı zaman aşımı değeri olabilir [UpdateMessage](/dotnet/api/microsoft.windowsazure.storage.queue.cloudqueue.updatemessage) API) |**Kuyruk düzeyi**<br/><br/>(her kuyruk iletileri tümüne uygulanan kilit kesinliği var, ancak kilit kullanarak yenileyebilirsiniz [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) API.) |
| Toplu olarak alma |**Evet**<br/><br/>(açıkça ileti sayısı en fazla 32 ileti kadar ileti alınırken belirtme) |**Evet**<br/><br/>(örtük olarak getirme öncesi özellik etkinleştirme veya işlemleri kullanarak açıkça) |
| Toplu gönderme |**Hayır** |**Evet**<br/><br/>(işlem veya istemci tarafı işlem grubu oluşturma kullanılarak) |

### <a name="additional-information"></a>Ek bilgiler
* Depolama kuyruğu iletilerinde genellikle ilk-giren ilk çıkar, ancak bazen sıralamaya olabilirler. Örneğin, bir iletinin görünebilirlik zaman aşımı süresi (örneğin, bir istemci uygulaması işleme sırasında kilitlenme) nedeniyle süresinin sona erdiği. Görünebilirlik zaman aşımı süresi dolduğunda, iletiyi sıradan çıkarma başka bir çalışan için kuyruktaki yeniden görünür hale gelir. Bu noktada, yeni görünür ileti sıraya alınan özgün sonra olduğu bir ileti sonra (yeniden sıradan çıkarılan için) sırada yerleştirilebilir.
* Service Bus kuyruklarını garantili FIFO desende Mesajlaşma oturumları gerektirir. Uygulama bir ileti alındığında işlenirken kilitleniyor olay içeren **Özet & Kilitle** modu, sonraki açışınızda bir kuyruk alıcı bir Mesajlaşma oturumunun kabul eder, başlayacak başarısız iletisiyle, yaşam süresi (TTL sonra) Dönem sona eriyor.
* Uygulama bileşenleri ayırma ölçeklenebilirlik ve dayanıklılık için hataları, artırmak için Yük Dengeleme ve süreç iş akışlarının oluşturulmasını gibi depolama kuyrukları standart sıraya alma senaryolarını desteklemek üzere tasarlanmıştır.
* Service Bus kuyrukları Destek *en az bir kez* teslim garantisi. 
* Service Bus oturumları bağlamında ileti işleme onaylamaz tutarsızlıklar oturumunun ileti sırası işleme ilerleme göre uygulamanın durumunu depolamak için oturum durumu kullanmanın ve işlemleri geçici bir çözüm kullanarak önlenebilir sonlandırma, iletileri ve oturum durumu güncelleştiriliyor aldı. Bu tür bir tutarlılık özellik bazen etiketli *tam olarak-bir kez işlenmesini* diğer satıcının ürünleri, ancak işlem hataları ileti yeniden teslim edilebilir açıkça neden olur ve bu nedenle terimi tam olarak yeterli.
* Depolama kuyrukları hem geliştiriciler hem de operasyon ekibi için kuyruklar, tablolar ve Blobları – arasında Tekdüzen ve tutarlı bir programlama modeli sağlar.
* Service Bus kuyrukları, yerel işlemler tek bir kuyruk için bağlamı desteği sunar.
* **Alma ve silme** Service Bus tarafından desteklenen modu azaltılmış teslim güvencesi lisanslarınıza Mesajlaşma işlemi sayısı (ve ilişkili maliyetin) azaltma olanağı sağlar.
* Depolama kuyrukları kiraları kiraları iletileri için genişletme olanağı sunar. Bu kısa kira iletilerinde korumak çalışanlar sağlar. Bir çalışanın kilitlenmesi durumunda, bu nedenle, ileti hızlı bir şekilde yeniden tarafından başka bir alt işlenebilir. Ayrıca, bir çalışan işlem sırasında geçerli kira süresini daha uzun gerektiriyorsa bir ileti kirayı genişletebilirsiniz.
* Depolama kuyrukları enqueuing ayarlayın ya da bir ileti kuyruktan çıkarma işlemlerini gerçekleştirebileceğiniz bir görünebilirlik zaman aşımı sağlar. Ayrıca, çalışma zamanında farklı kira değerlerle bir ileti güncelleştirin ve aynı sıradaki iletiler arasında farklı değerlerini güncelleştirin. Service Bus kilit zaman aşımı kuyruk meta verilerde tanımlanan; Ancak, çağırarak kilidi yenileme yapabilir [RenewLock](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.renewlock#Microsoft_ServiceBus_Messaging_BrokeredMessage_RenewLock) yöntemi.
* Hizmet veri yolu kuyrukları bir engelleme alma işlemi için en uzun zaman aşımı 24 gündür. Ancak, REST tabanlı bir zaman aşımı 55 saniye maksimum değerine sahip.
* Hizmet veri yolu tarafından sağlanan istemci-tarafı toplu işleme, birden çok ileti toplu bir tek gönderme işlemi bir kuyruk istemci sağlar. Toplu işleme yalnızca zaman uyumsuz gönderme işlemleri için kullanılabilir.
* Depolama kuyrukları (daha fazla hesapları sanallaştırmak olduğunda) ve sınırsız kuyrukları, 200 TB tavan gibi özellikleri SaaS sağlayıcıları için ideal bir platform kolaylaştırır.
* Depolama kuyrukları bir esnek sağlayın ve yüksek performanslı, erişim denetimi mekanizmasını temsilcisi.

## <a name="advanced-capabilities"></a>Gelişmiş Özellikler
Bu bölümde, depolama kuyrukları ve Service Bus kuyrukları tarafından sağlanan gelişmiş özellikleri karşılaştırır.

| Karşılaştırma ölçütleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Zamanlanmış teslim |**Evet** |**Evet** |
| Otomatik geçersiz yazımın ardından gerçekleşen |**Hayır** |**Evet** |
| Artan sıra yaşam süresi değeri |**Evet**<br/><br/>(yerinde güncelleştirme / görünebilirlik zaman aşımı) |**Evet**<br/><br/>(bir özel API işlevi sağlanır) |
| Zehirli ileti desteği |**Evet** |**Evet** |
| Yerinde güncelleştirme |**Evet** |**Evet** |
| Sunucu tarafı işlem günlüğü |**Evet** |**Hayır** |
| Depolama ölçümleri |**Evet**<br/><br/>**Dakika ölçümleri**: Gerçek zamanlı kullanılabilirlik, TPS, API ölçümlerini çağrı sayılarını, hata sayıları ve diğer tüm gerçek zamanlı olarak (dakika başına toplanır ve üretimde ne mi oldu gelen birkaç dakika içinde bildirilen. sağlar Daha fazla bilgi için [Storage Analytics ölçümleri hakkında](/rest/api/storageservices/fileservices/About-Storage-Analytics-Metrics). |**Evet**<br/><br/>(toplu sorgular çağırarak [GetQueues](/dotnet/api/microsoft.servicebus.namespacemanager.getqueues#Microsoft_ServiceBus_NamespaceManager_GetQueues)) |
| Durum yönetimi |**Hayır** |**Evet**<br/><br/>[Microsoft.ServiceBus.Messaging.EntityStatus.Active](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.Disabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.SendDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus), [Microsoft.ServiceBus.Messaging.EntityStatus.ReceiveDisabled](/dotnet/api/microsoft.servicebus.messaging.entitystatus) |
| Otomatik iletme iletisi |**Hayır** |**Evet** |
| Kuyruk işlevi Temizle |**Evet** |**Hayır** |
| İleti grupları |**Hayır** |**Evet**<br/><br/>(ileti oturumları kullanarak) |
| İleti grubu başına uygulama durumu |**Hayır** |**Evet** |
| Yineleme algılama |**Hayır** |**Evet**<br/><br/>(Gönderen tarafında yapılandırılabilir) |
| İleti grupları göz atma |**Hayır** |**Evet** |
| Mesaj oturumları Kimliğine göre getiriliyor |**Hayır** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Daha sonra teslim edilmek üzere zamanlanmış bir ileti kuyruğa alma hem teknolojiler sağlar.
* Kuyruk otomatik iletme otomatik iletme kendi kendisinden alıcı uygulamanın iletiyi tüketir, tek bir kuyruk iletileri için kuyrukları binlerce sağlar. Bu mekanizma kullanarak güvenliği sağlamak, akışını denetlemek için ve her ileti yayımcı arasında depolama alanı ayır.
* Depolama kuyruklarına ileti içeriği güncelleştirmek için destek sağlar. Böylece sıfırdan başlıyor yerine en son bilinen kontrol noktasından işlenebilir bu işlevselliği sürüyor durumu bilgileri ve artımlı ilerleme güncelleştirmeleri için iletiye kullanabilirsiniz. Service Bus kuyrukları ile aynı senaryo ileti oturumları kullanarak etkinleştirebilirsiniz. Oturumları etkinleştir, kaydetme ve uygulama işleme durumunu almak (kullanarak [SetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.setstate#Microsoft_ServiceBus_Messaging_MessageSession_SetState_System_IO_Stream_) ve [GetState](/dotnet/api/microsoft.servicebus.messaging.messagesession.getstate#Microsoft_ServiceBus_Messaging_MessageSession_GetState)).
* [Kullanılmayan lettering](service-bus-dead-letter-queues.md), olduğu yalnızca hizmet veri yolu kuyrukları tarafından desteklenen, işlenemiyor başarıyla alıcı uygulama tarafından veya ne zaman iletileri ulaşamıyor hedeflerine süresi dolmuş bir yaşam süresi (nedeniyle ileti yalıtmak için yararlı olabilir TTL) özelliği. TTL değeri ne kadar bir ileti kuyruğunda kalır belirtir. Service Bus ile TTL süresi sona erdiğinde $DeadLetterQueue adlı özel bir kuyruğa ileti taşınır.
* "Zehirli" iletileri depolama kuyruğunda bir ileti kuyruktan çıkarma işlemlerini zaman uygulama bulmak için inceler [DequeueCount](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.queue.cloudqueuemessage.dequeuecount.aspx) iletinin özelliği. Varsa **DequeueCount** belirli bir eşikten büyük uygulama, bir uygulama tarafından tanımlanan "geçersiz" sırasına iletiyi taşır.
* Depolama kuyrukları tüm düzgün olarak toplanan ölçümler olarak sıranın karşı yürütülen işlemlerin ayrıntılı günlüğü almak için etkinleştirin. Bu iki seçenek, hata ayıklama ve uygulamanızın depolama kuyrukları nasıl kullandığı anlamak için kullanışlıdır. Ayrıca, uygulamanızın performans ayarlama ve kuyrukları kullanma maliyetini azaltmak için yararlıdır.
* İletiler içeren ve bunların ilgili alıcılar arasında oturum benzeri bir benzeşim sırayla oluşturan belirli bir alıcı ile ilişkilendirilecek belirli bir mantıksal gruba ait iletileri "Service Bus tarafından desteklenen mesaj oturumları" kavramı sağlar. Bu hizmet veri yolu işlevselliğini ayarlayarak Gelişmiş etkinleştirebilirsiniz [SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sessionid#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) bir ileti özelliği. Alıcılar daha sonra belirli bir oturum Kimliğinde belirtilen oturum tanımlayıcısını paylaşan ileti dinleyip almak.
* Bir kuyruk veya konuda değerine göre gönderilen yinelenen iletileri otomatik olarak Service Bus kuyrukları ile desteklenen yineleme algılama işlevi kaldırır [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.messageid#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) özelliği.

## <a name="capacity-and-quotas"></a>Kapasiteyi ve kotayı
Bu bölümde, depolama kuyrukları ve Service Bus kuyrukları açısından karşılaştırır [kapasiteyi ve kotayı](service-bus-quotas.md) geçerli olabilir.

| Karşılaştırma ölçütleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| En büyük sıra boyutu |**500 TB**<br/><br/>(sınırlı bir [tek bir depolama hesabı kapasitesi](../storage/common/storage-introduction.md#queue-storage)) |**80 GB için 1 GB**<br/><br/>(bir kuyruğun oluşturulduktan sonra tanımlanmış ve [bölümleme etkinleştirme](service-bus-partitioning.md) – "Ek bilgiler" bölümüne bakın) |
| En büyük ileti boyutu |**64 KB**<br/><br/>(48 kullanırken KB **Base64** kodlaması)<br/><br/>Azure kuyrukları ve blobları – bu noktada, şunları yapabilirsiniz kuyruğa birleştirerek büyük iletileri destekleyen tek bir öğe için en fazla 200 GB. |**256 KB** veya **1 MB**<br/><br/>(hem üst hem de gövde, en fazla üstbilgi boyutu da dahil olmak üzere: 64 KB).<br/><br/>Bağımlı [hizmet katmanı](service-bus-premium-messaging.md). |
| En fazla ileti TTL |**Sonsuz** (itibariyle API Sürüm 2017-07-27) |**TimeSpan.Max** |
| En yüksek sayıda sıra |**Sınırsız** |**10,000**<br/><br/>(hizmet ad alanı) |
| Maksimum eşzamanlı istemci sayısı |**Sınırsız** |**Sınırsız**<br/><br/>(yalnızca 100 eşzamanlı bağlantı sınırı TCP protokolü tabanlı iletişim için'geçerlidir) |

### <a name="additional-information"></a>Ek bilgiler
* Service Bus kuyruk boyutu sınırlarındaki zorlar. En büyük sıra boyutu kuyruğun oluşturma sırasında belirtilir ve 1 ile 80 GB arasında bir değer olabilir. Sıranın oluşturulmasını üzerinde ayarlanan kuyruk boyutu değeri ulaşılırsa ek gelen iletiler reddedilir ve bir özel durum çağıran kod tarafından alınır. Service Bus kotaları hakkında daha fazla bilgi için bkz: [Service Bus kotaları](service-bus-quotas.md).
* Bölümleme desteklenmez [Premium katmanı](service-bus-premium-messaging.md). Standart katmanda, hizmet veri yolu kuyrukları (varsayılan 1 GB'tır) boyutları 1, 2, 3, 4 veya 5 GB oluşturabilirsiniz. Standart katman, bölümleme ile etkin (varsayılan seçenek), Service Bus, belirttiğiniz her bir GB için 16 bölümler oluşturur. 5 GB boyutundadır bir kuyruk oluşturmak, bu nedenle, 16 bölüm en büyük sıra boyutu (5 * 16) haline gelir = 80 GB. Bölümlenmiş bir kuyruk veya konuda en büyük boyutunu, girdi bakarak gördüğünüz [Azure portalında][Azure portal].
* Depolama kuyrukları ile iletisinin içeriği XML uyumlu değilse, bu olmalıdır **Base64** kodlanmış. Varsa, **Base64**-iletisine kodlayın, kullanıcı yükü 64 KB yerine en fazla 48 KB olabilir.
* Service Bus kuyrukları ile depolanan bir kuyruğa her ileti iki bölümden oluşur: bir başlık ve gövde. İletilerin toplam boyutu için hizmet katmanı tarafından desteklenen en büyük ileti boyutunu aşamaz.
* İstemciler TCP protokolü üzerinden Service BUS ile iletişim kurarken, tek bir Service Bus kuyruğuna eş zamanlı bağlantı sayısı 100'e sınırlıdır. Bu sayı göndericiler ile alıcılar arasında paylaşılır. Bu kotasına ulaşıldığında, ek bağlantı için sonraki istekler reddedilir ve bir özel durum çağıran kod tarafından alınır. REST tabanlı API kullanarak kuyruklarına bağlanan istemcilerde bu sınırı uygulanmaktadır değil.
* 10. 000'den fazla kuyrukları tek bir Service Bus ad alanında ihtiyacınız varsa Azure Destek ekibine başvurun ve artışı isteyebilir. Service Bus ile 10.000 kuyrukları ötesine genişletmek için de ek ad alanlarını kullanarak oluşturabilirsiniz [Azure portalında][Azure portal].

## <a name="management-and-operations"></a>Yönetim ve işlemler
Bu bölümde, depolama kuyrukları ve Service Bus kuyrukları tarafından sağlanan yönetim özellikleri karşılaştırılır.

| Karşılaştırma ölçütleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Yönetim Protokolü |**HTTP/HTTPS üzerinden REST** |**HTTPS üzerinden REST** |
| Çalışma zamanı Protokolü |**HTTP/HTTPS üzerinden REST** |**HTTPS üzerinden REST**<br/><br/>**AMQP 1.0 Standart (TCP TLS ile)** |
| .NET API’si |**Evet**<br/><br/>(.NET depolama istemcisi API'sini) |**Evet**<br/><br/>(.NET Service Bus API) |
| Yerel C++ |**Evet** |**Evet** |
| Java API’si |**Evet** |**Evet** |
| PHP API |**Evet** |**Evet** |
| Node.js API'si |**Evet** |**Evet** |
| Rastgele meta veri desteği |**Evet** |**Hayır** |
| Kuyruk adlandırma kuralları |**En fazla 63 karakter uzunluğunda**<br/><br/>(Bir kuyruk adı harfler küçük harfle yazılmalıdır.) |**En fazla 260 karakter uzunluğunda**<br/><br/>(Sıra yollarını ve adlarını büyük küçük harf duyarlıdır.) |
| Get kuyruk uzunluğu işlevi |**Evet**<br/><br/>(Silinmiş olmadan TTL iletilerin süresi dolar yaklaşık değerini.) |**Evet**<br/><br/>(Değer. tam, zaman içinde nokta) |
| Gözlem işlevi |**Evet** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Depolama kuyrukları, ad/değer çiftleri biçiminde kuyruk açıklamasını uygulanabilir rastgele öznitelikler için destek sağlar.
* Hem kuyruk teknolojileri, queue explorer/tarayıcı aracı uygularken yararlı olabilen kilitleme zorunda kalmadan bir iletiye olanağı sunar.
* Hizmet veri yolu .NET aracılı Mesajlaşma API'lerini yararlanın tam çift yönlü TCP bağlantılar HTTP üzerinden REST karşılaştırıldığında geliştirilmiş performans ve standart AMQP 1.0 protokolünü destekler.
* Depolama kuyrukları adları 3 ila 63 karakter uzunluğunda olabilir, küçük harf, sayı ve kısa çizgi içerebilir. Daha fazla bilgi için [adlandırma kuyrukları ve meta verileri](/rest/api/storageservices/fileservices/Naming-Queues-and-Metadata).
* Service Bus kuyruk adları, en fazla 260 karakter uzunluğunda olabilir ve daha az kısıtlayıcı adlandırma kuralları vardır. Service Bus kuyruk adları harf, rakam, nokta, kısa çizgi ve alt çizgi içerebilir.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme
Bu bölümde, depolama kuyrukları ve Service Bus kuyrukları tarafından desteklenen kimlik doğrulama ve yetkilendirme özellikleri açıklanmaktadır.

| Karşılaştırma ölçütleri | Depolama kuyrukları | Service Bus kuyrukları |
| --- | --- | --- |
| Authentication |**Simetrik anahtar** |**Simetrik anahtar** |
| Güvenlik modeli |Temsilcili erişim aracılığıyla SAS belirteçleri. |SAS |
| Kimlik sağlayıcısı Federasyonu |**Hayır** |**Evet** |

### <a name="additional-information"></a>Ek bilgiler
* Sıraya alma teknolojilerinin ya da her isteğin kimliğinin doğrulanması gerekir. Anonim erişimi olan genel sıralar desteklenmez. Kullanarak [SAS](service-bus-sas.md), bu senaryo bir salt okunur SAS, salt okunur SAS veya tam erişim SAS yayımlayarak karşılayabilirsiniz.
* Kimlik doğrulama şeması tarafından depolama kuyrukları içerir sağlanan bir karma tabanlı ileti kimlik doğrulama kodu (HMAC) olan bir simetrik anahtar kullanımı ile SHA-256 algoritmasını hesaplanan ve olarak kodlanmış bir **Base64** dize. İlgili protokolü hakkında daha fazla bilgi için bkz. [kimlik doğrulaması için Azure Storage Hizmetleri](/rest/api/storageservices/fileservices/Authentication-for-the-Azure-Storage-Services). Service Bus kuyrukları, simetrik anahtarlar kullanılarak benzer bir modeli destekler. Daha fazla bilgi için [paylaşılan erişim imzası kimlik doğrulaması ile Service Bus](service-bus-sas.md).

## <a name="conclusion"></a>Sonuç
İki teknolojiyi daha derin bir anlayış kazanmak tarafından kullanmak için hangi sıranın teknolojisini daha bilinçli kararlar mümkün olmayacak ve ne zaman. Depolama kuyruğu veya Service Bus kuyruklarını ne zaman kullanılacağını kararını açıkça bir dizi faktöre bağlıdır. Bu etkenler, yoğun ihtiyaçlara uygulamanız ve mimarisinin bağlı olabilir. Uygulamanız zaten Microsoft Azure'un temel özellikleriyle kullanıyorsa, özellikle temel iletişim ve mesajlaşma Hizmetleri veya boyutu 80 GB'tan daha büyük olabilir gerek sıraları arasında gerekiyorsa, depolama kuyruklarına, seçmek tercih edebilirsiniz.

Service Bus kuyrukları birkaç oturumları, hareketler gibi gelişmiş özellikler sunar çünkü otomatik olan algılama yinelenen ulaşmayan ve dayanıklı yayımlama/abonelik yetenekleri, bunlar bir tercih edilen seçim olabilir karma oluşturuyorsanız uygulama veya aksi halde, uygulamanız bu özellikler gerektiriyorsa.

## <a name="next-steps"></a>Sonraki adımlar
Aşağıdaki makaleler, daha fazla rehberlik ve depolama kuyruğu veya Service Bus kuyruklarını kullanma hakkında bilgi sağlar.

* [Service Bus kuyrukları ile çalışmaya başlama](service-bus-dotnet-get-started-with-queues.md)
* [Kuyruk depolama hizmetini kullanma](../storage/queues/storage-dotnet-how-to-use-queues.md)
* [Service Bus'ı kullanarak performans geliştirme en iyi aracılı Mesajlaşma](service-bus-performance-improvements.md)
* [Kuyruklar ve konular, Azure Service Bus (blog gönderisi) ile tanışın](https://www.code-magazine.com/article.aspx?quickid=1112041)
* [Hizmet veri yolu için Geliştirici Kılavuzu](http://www.cloudcasts.net/devguide/Default.aspx?id=11030)
* [Sıraya alma hizmetidir Azure'da kullanma](https://www.developerfusion.com/article/120197/using-the-queuing-service-in-windows-azure/)

[Azure portal]: https://portal.azure.com

