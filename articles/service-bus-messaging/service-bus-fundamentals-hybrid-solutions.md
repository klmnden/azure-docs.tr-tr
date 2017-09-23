---
title: "Azure Service Bus ile ilgili temel bilgilere genel bakış | Microsoft Docs"
description: "Azure uygulamalarını başka bir yazılıma bağlamak için Service Bus kullanımına giriş."
services: service-bus-messaging
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 12654cdd-82ab-4b95-b56f-08a5a8bbc6f9
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 06/15/2017
ms.author: sethm
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: af8b10f0a460e695a39879718174e81f78934ef8
ms.contentlocale: tr-tr
ms.lasthandoff: 06/17/2017

---
# <a name="azure-service-bus"></a>Azure Service Bus

Bir uygulama veya hizmet ister bulutta ister şirket içinde çalışsın, genellikle diğer uygulamalarla veya hizmetlerle etkileşimde olmalıdır. Bunu gerçekleştirmenin oldukça kapsamlı ve kullanışlı bir yöntemi ise Microsoft Azure tarafından sunulan Service Bus hizmetidir. Bu makalede, bu teknoloji genel hatlarıyla incelenmiş olup söz konusu teknolojinin yapısına ve bu teknolojiyi kullanmanız durumunda elde edeceğiniz avantajlara yer verilmiştir.

## <a name="service-bus-fundamentals"></a>Service Bus ile ilgili temel bilgiler

Farklı çözümler, farklı iletişim stilleri gerektirir. Bazı durumlarda, uygulamaların basit bir kuyruk aracılığıyla iletileri almasına veya göndermesine izin vermek en iyi çözümdür. Diğer durumlarda ise sıradan bir kuyruk yeterli değildir; yayımla ve abone ol mekanizmasını içeren bir kuyruk daha faydalıdır. Bazı durumlarda uygulamalar arasında bağlantı olması yeterlidir ve kuyruklara gerek yoktur. Service Bus, uygulamalarınızın birçok farklı yöntemle etkileşim kurmasına olanak sağlayarak bu seçeneklerin üçünü de sunar.

Service Bus çok kiracılı bir bulut hizmetidir, yani hizmet birçok kullanıcı tarafından paylaşılır. Her kullanıcı (örneğin, bir uygulama geliştiricisi) bir *ad alanı* oluşturur ve bu ad alanında ihtiyaç duyacağı iletişim mekanizmasını tanımlar. Şekil 1'de bu mimarinin nasıl göründüğü gösterilmiştir.

![][1]

**Şekil 1: Service Bus, bulut üzerinden uygulamaları bağlamak için çok kiracılı bir hizmet sunar.**

Ad alanı içinde, üç farklı iletişim mekanizmasının bir veya birden fazla örneğini kullanabilirsiniz. Bu mekanizmaların her biri, uygulamaları farklı yöntemlerle bağlar. Seçenekler şunlardır:

* Tek yönlü iletişime izin veren *kuyruklar*. Her kuyruk, alınana kadar gönderilen iletileri depolayan bir ara hizmet olarak görev yapar (bazen *aracı* olarak adlandırılır). Her ileti tek bir alıcı tarafından alınır.
* *Abonelikler* olmak üzere tek konu başlığı kullanan, tek yönlü iletişim sağlayan *Konu Başlıkları*, birden çok aboneliği barındırabilir. Kuyrukta olduğu gibi konu başlığı, aracı gibi davranır ancak her abonelik isteğe bağlı olarak yalnızca belirli kriterlerle eşleşen iletileri almak için filtre kullanabilir.
* Çift yönlü iletişim sunan *geçişler*. Kuyruk ve konu başlıklarının aksine, bir geçiş yürütülen iletileri depolamaz; yani bir aracı değildir. Bunun yerine iletileri yalnızca hedef uygulamaya gönderir.

Bir kuyruk, konu veya geçiş oluşturduğunuzda bunları adlandırırsınız. Ad alanınız ne olursa olsun, bu ad nesne için benzersiz bir tanıtıcı oluşturur. Uygulamalar bu adı Service Bus'a verebilir ve ardından birbirleriyle iletişim kurmak için bu kuyruğu, konuyu veya geçişi kullanabilir. 

Geçiş senaryosundaki bu nesnelerden herhangi birini kullanmak için Windows uygulamaları Windows Communication Foundation'ı (WCF) kullanabilir. Bu hizmet [WCF Geçişi](../service-bus-relay/relay-what-is-it.md) olarak bilinir. Windows uygulamaları kuyruklar ve konular için Service Bus tanımlı mesajlaşma API'lerini kullanabilir. Bu nesnelerin Windows uygulaması olmayan uygulamalar tarafından kullanımını kolaylaştırmak için Microsoft Java, Node.js ve diğer dillere yönelik SDK sunar. Kuyruklara ve konu başlıklarına HTTP(s) üzerinden [REST API'lerini](/rest/api/servicebus/) kullanarak da erişebilirsiniz. 

Service Bus hizmeti bulutta (Microsoft'un Azure veri merkezlerinde) çalışıyor olsa da Service Bus hizmetini kullanan uygulamaların herhangi bir yerde çalışabileceğini kavramak önemlidir. Service Bus hizmetini Azure'da çalışan uygulamaları (örneğin, kendi veri merkezinizde çalışan uygulamalar) bağlamak için kullanabilirsiniz . Ayrıca, bu hizmeti Azure veya başka bulut platformunda çalışan bir uygulama ile şirket içi bir uygulamayı veya tablet ve telefonları bağlamak için de kullanabilirsiniz. Ev aletlerini, sensörleri ve diğer cihazları merkezi bir uygulamaya veya başka bir uygulamaya bağlamak bile mümkündür. Service Bus, neredeyse her yerden erişilebilen bulut tabanlı bir iletişim mekanizmasıdır. Service Bus hizmetini kullanım şekliniz uygulamanızın gereksinimlerine göre değişir.

## <a name="queues"></a>Kuyruklar

İki uygulamayı Service Bus kuyruğu kullanarak bağlamaya karar verdiğinizi varsayalım. Şekil 2'de bu durum gösterilir.

![][2]

**Şekil 2: Service Bus kuyrukları tek yönlü, zaman uyumsuz kuyruğa alma işlemi sunar.**

İşlem oldukça basittir: Bir gönderici Service Bus kuyruğuna ileti gönderir ve daha sonra alıcı bu iletiyi alır. Kuyruk Şekil 2'de gösterildiği gibi yalnızca tek bir alıcıya sahip olabilir. Ya da aynı kuyruktan birden fazla uygulama okuyabilir. İkinci durumda her ileti yalnızca bir alıcı tarafından okunur. Çok noktaya yayın hizmetinde bunun yerine bir konu kullanmanız gerekir.

Her ileti iki bölümden oluşur: özellikler kümesi (her biri anahtar/değer çifti olmak üzere) ve ileti yükü. Yük ikili değer, metin, hatta XML olabilir. Bunların nasıl kullanıldığı, uygulama tarafından gerçekleştirilmeye çalışılan işleme bağlıdır. Örneğin, yakın zamandaki bir satış hakkında bir ileti gönderen uygulama **Seller="Ava"** ve **Amount=10000** özelliklerini içerebilir. İleti gövdesi, satışın imzalı sözleşmesinin taranmış bir görüntüsünü içerebilir veya görüntü mevcut değilse bu kısım boş bırakılır.

Bir alıcı iki farklı şekilde Service Bus kuyruğundaki iletileri okuyabilir. *[ReceiveAndDelete](/dotnet/api/microsoft.servicebus.messaging.receivemode)* olarak adlandırılan ilk seçenek, kuyruktan iletiyi kaldırır ve anında siler. Bu seçenek basittir ancak alıcı, iletiyi işlemeyi tamamlamadan bir çökme gerçekleşirse ileti kaybolur. Kuyruktan kaldırılmış olduğundan başka bir alıcı bu iletiye erişemez. 

*[PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode)* olan ikinci seçenek ise bu soruna çözüm bulmak için tasarlanmıştır. **ReceiveAndDelete** gibi, **PeekLock** yöntemindeki okuma işlemi de iletiyi kuyruktan kaldırır. Ancak iletiyi silmez. Bunun yerine, iletiyi kilitleyerek diğer alıcılar için görünmez yapar ve aşağıdaki olaylardan birinin gerçekleşmesini bekler:

* Alıcı iletiyi başarıyla işlediğinde [Complete()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Complete) çağrısı yapar ve kuyruk iletiyi siler. 
* Alıcı, iletiyi başarıyla işleyemediğine karar verirse [Abandon()](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Abandon) çağrısını yapar. Daha sonra kuyruk iletinin kilidini açar ve iletiyi diğer alıcılar için kullanılabilir hale getirir.
* Alıcı, yapılandırılabilir bir süre içinde (varsayılan olarak 60 saniyedir) bu yöntemlerden hiçbirini çağırmazsa kuyruk, alıcının başarısız olduğunu varsayar. Bu durumda, alıcının **Abandon** çağrısı yaptığını varsayarak hareket eder ve iletiyi diğer alıcılar için kullanılabilir hale getirir.

Burada gerçekleşebilecek şu duruma dikkat edin: Aynı ileti iki kez (belki de iki farklı alıcıya) teslim edilebilir. Service Bus kuyruklarını kullanan uygulamalar bu durum için hazırlıklı olmalıdır. Yinelenen öğe algılamasını daha kolay hale getirmek için her iletinin benzersiz [MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) özelliği vardır. Bu özellik, iletinin kuyruktan kaç kez okunduğuna bakılmaksızın varsayılan olarak sürekli aynıdır. 

Kuyruklar birçok durumda oldukça faydalıdır. Kuyruklar sayesinde aynı anda çalışmayan uygulamaların bile iletişim kurmasına olanak sağlanır; özellikle toplu işlem ve mobil uygulamalarda olmak üzere bu özellik oldukça kullanışlıdır. Ayrıca, birden çok alıcısı bulunan bir kuyruk otomatik olarak yük dengelemesi sunar. Bu durum, gönderilen iletilerin tüm alıcılara dağıtılmasından kaynaklanır.

## <a name="topics"></a>Konu başlıkları

Ne kadar faydalı olsalar da kuyruklar her zaman doğru çözüm değildir. Bazı durumlarda Service Bus konu başlıkları daha faydalıdır. Şekil 3'te bu durum gösterilir.

![][3]

**Şekil 3: Uygulama, abone uygulamanın belirlediği bir filtreye bağlı olarak Service Bus konu başlığına gönderilen iletilerin bazılarını veya tümünü alabilir.**

*Konu başlığı* birçok açıdan kuyruğa benzer. Göndericiler, iletileri kuyruğa gönderdikleri gibi aynı şekilde konu başlığına gönderir ve bu iletiler kuyrukta göründükleri gibi görünür. Aradaki fark ise konu başlıklarının, alıcı uygulamaların her birinin bir *filtre* belirleyerek kendi *aboneliklerini* oluşturmasına olanak sağlamasıdır. Böylece abone yalnızca filtreyle eşleşen iletileri görebilir. Örneğin, Şekil 3'te, bir gönderici ile üç abonesi bulunan bir konu başlığı ve abonelerin her birinin kendi filtrelerine sahip olduğu bir durum gösterilir:

* Abone 1 yalnızca *Seller="Ava"* özelliğini içeren iletileri alır.
* Abone 2 ise *Seller="Ruby"* ve/veya değeri 100.000'den fazla olan *Amount* özelliklerini içeren iletileri alır. Satış müdürü olduğunu varsaydığımız Ruby, hem kendi satışlarını hem de kimin yaptığı fark etmeksizin tüm büyük satışları görmek isteyebilir.
* Abone 3, filtresini *True* olarak ayarlar ve tüm iletileri alır. Örneğin, bu uygulama bir denetim kaydı tutmakla görevlendirilmiştir ve tüm iletileri görmesi gerekir.

Kuyruklarda olduğu gibi, bir konu başlığının aboneleri de iletileri [ReceiveAndDelete veya PeekLock](/dotnet/api/microsoft.servicebus.messaging.receivemode) kullanarak okuyabilir. Ancak kuyrukların aksine, konu başlığına gönderilen tek bir ileti birden çok abonelik tarafından alınabilir. Yaygın şekilde *yayımla ve abone ol* (veya *pub/sub*) olarak adlandırılan bu yaklaşım, aynı iletilerin birden çok uygulamanın ilgi alanına girmesi durumunda faydalıdır. Her abone, doğru filtreyi tanımlayarak ileti akışının yalnızca görmesi gereken kısmını seçebilir.

## <a name="relays"></a>Geçişler

Hem kuyruklar hem de konu başlıkları, bir aracı yoluyla tek yönlü zaman uyumsuz iletişim sağlar. Trafik akışları sadece tek yöndedir ve göndericiler ile alıcılar arasında doğrudan bağlantı yoktur. Peki bu bağlantıyı istemiyorsanız ne yapmanız gerekir? Uygulamalarınızın iletileri hem göndermesi hem de alması gerektiğini ya da gönderici ile alıcılar arasında doğrudan bağlantı istediğinizi ve iletileri depolamak için aracıya ihtiyacınız olmadığını düşünelim. Bunun gibi bir senaryoya uyum sağlamak için Service Bus, Şekil 4'te gösterildiği gibi *geçişleri* kullanır.

![][4]

**Şekil 4: Service Bus geçişi, uygulamalar arasında zaman uyumlu, çift yönlü iletişim sağlar.**

Geçiş kullanımı hakkında akla gelen ilk soru ise şudur: Neden geçiş kullanmalıyım? Kuyruklara ihtiyacım olmasa bile doğrudan etkileşim sağlamak yerine uygulamaların arasındaki iletişimi neden bir bulut hizmeti aracılığıyla sağlamalıyım? Bu soruya verilen cevap ise doğrudan iletişim kurmanın düşündüğünüzden daha zor olmasıdır.

Her ikisi de kurumsal veri merkezlerinde çalışan iki şirket içi uygulama arasında bağlantı kurmak istediğinizi düşünelim. Bu uygulamaların her biri güvenlik duvarının arkasında bulunur ve her veri merkezi de ağ adresi çevirisi (NAT) kullanır. Güvenlik duvarı, birkaçı dışındaki tüm bağlantı noktalarından gelen verileri engeller ve NAT ise uygulamaların çalıştığı makinelerin veri merkezi dışından doğrudan erişebileceğiniz sabit bir IP adresi olmadığını ifade eder. İlave yardım almadan genel İnternet üzerinden bu uygulamaları bağlamak sorun yaratır.

Bir Service Bus geçişi yararlı olabilir. Bir geçiş aracılığıyla çift yönlü iletişim sağlamak için her uygulama Service Bus içeren bir giden TCP bağlantısı kurar ve bu bağlantıyı açık tutar. İki uygulama arasındaki tüm iletişim bu bağlantılar üzerinden kurulur. Tüm bağlantılar veri merkezi içinden kurulduğundan, güvenlik duvarı her uygulama için gelen trafiğe yeni bağlantı noktaları açmadan izin verir. Ayrıca, her uygulama iletişim boyunca bulutta sabit bir uç noktaya sahip olduğundan, bu çözüm NAT sorununu da ortadan kaldırır.  Uygulamalar, geçiş aracılığıyla veri değişimi yaparak gerçekleşmeleri durumunda iletişimi zorlaştıracak sorunlardan kaçınabilir. 

Service Bus geçişlerini kullanmak için uygulamalar, Windows Communication Foundation'a (WCF) güvenir. Service Bus, Windows uygulamaları için geçişler aracılığıyla etkileşim sağlamayı doğrudan olacak şekilde ayarlayan WCF bağlamalarını sunar. WCF'yi zaten kullanmakta olan uygulamalar genel olarak bu bağlamaların bir tanesini belirtebilir ve ardından bir geçiş aracılığıyla birbirleriyle iletişim kurabilir. Öte yandan kuyrukların ve konu başlıklarının aksine, olası durumlarda Windows uygulaması olmayan uygulamalarda geçişlerin kullanılması standart kitaplıklar sağlanmadığından programlama açısından biraz daha fazla çaba sarf edilmesini gerektirir.

Kuyruk ve konu başlıklarının aksine uygulamalar, geçişleri açık bir şekilde oluşturmaz. Bunun yerine, iletileri almak isteyen bir uygulama Service Bus içeren bir TCP bağlantısı kurduğunda otomatik olarak geçiş oluşturulur. Bağlantı kesildiğinde geçiş de silinir. Uygulamanın belirli bir dinleyici tarafından oluşturulan geçişi bulması için Service Bus, uygulamaların ada göre belirli geçişleri bulmalarına olanak sağlayan bir kayıt defteri sunar.

Uygulamalar arasında doğrudan iletişim kurulması gerekiyorsa geçişler doğru çözümdür. Örneğin; check-in kiosk cihazları, mobil cihazlar ve diğer bilgisayarlardan erişilebilmesi gereken ve bir şirket içi veri merkezinde çalışan hava yolu rezervasyon sistemini ele alalım. Bu sistemlerin tümünde çalışan uygulamalar, nerede çalıştıklarından bağımsız olarak iletişim kurmak için buluttaki Service Bus geçişlerine güvenebilir.

## <a name="summary"></a>Özet

Uygulamalar arasında bağlantı kurma, her zaman eksiksiz çözüm oluşturmanın bir parçası olmuştur. Bunun yanı sıra, İnternet'e bağlı uygulama ve cihaz sayısı arttıkça uygulamaların ve hizmetlerin birbirleriyle iletişim kurmasını gerektiren senaryoların sayısı da artmaktadır. Kuyruklar, konu başlıkları ve geçişler aracılığıyla iletişimin sağlanmasına yönelik bulut tabanlı teknolojiler sağlayan Service Bus, bu temel işlevin uygulanmasını kolaylaştırmayı ve daha geniş kapsamda kullanılmasını amaçlamaktadır.

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Service Bus hizmeti ile ilgili temel bilgileri edindiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

* [Service Bus kuyruklarını](service-bus-dotnet-get-started-with-queues.md) kullanma
* [Service Bus konu başlıklarını](service-bus-dotnet-how-to-use-topics-subscriptions.md) kullanma
* [Service Bus geçişini](../service-bus-relay/service-bus-dotnet-how-to-use-relay.md) kullanma
* [Service Bus örnekleri](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png

