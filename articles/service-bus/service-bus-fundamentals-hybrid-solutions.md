<properties 
    pageTitle="Azure Service Bus | Microsoft Azure" 
    description="Azure uygulamalarını başka bir yazılıma bağlamak için Service Bus kullanımına ilişkin farklı yöntemlere giriş." 
    services="service-bus" 
    documentationCenter=".net" 
    authors="sethmanheim" 
    manager="timlt" 
    editor=""/>

<tags 
    ms.service="service-bus" 
    ms.workload="na" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="get-started-article" 
    ms.date="03/09/2016" 
    ms.author="sethm"/>

# Azure Service Bus

Bir uygulama veya hizmet ister bulutta ister şirket içinde çalışsın, genellikle diğer uygulamalarla veya hizmetlerle etkileşimde olmalıdır. Bunu gerçekleştirmenin oldukça kapsamlı ve kullanışlı bir yöntemi ise Microsoft Azure tarafından sunulan Service Bus hizmetidir. Bu makalede, hizmetin yapısına ve hizmeti kullanmanız durumunda elde edeceğiniz avantajlara yer verilerek bu teknoloji genel hatlarıyla incelenir.

## Service Bus ile ilgili temel bilgiler

Farklı çözümler, farklı iletişim stilleri gerektirir. Bazı durumlarda, uygulamaların basit bir kuyruk aracılığıyla iletileri almasına veya göndermesine izin vermek en iyi çözümdür. Diğer durumlarda ise sıradan bir kuyruk yeterli değildir; yayımla ve abone ol mekanizmasını içeren bir kuyruk daha faydalıdır. Gereken tek şeyin uygulamalar arasında kurulan bağlantı olduğu ve kuyrukların gerekmediği durumlar da mevcuttur. Service Bus, uygulamalarınızın birçok farklı yöntemle etkileşim kurmasına olanak sağlayarak bu seçeneklerin üçünü de sunar.

Service Bus çok kiracılı bir bulut hizmetidir, yani hizmet birçok kullanıcı tarafından paylaşılır. Her kullanıcı (örneğin, uygulama geliştiricisi) bir *ad alanı* oluşturur ve ardından ad alanında ihtiyaç duyacağı iletişim mekanizmasını tanımlar. Şekil 1'de bu sistem gösterilmektedir.

![][1]
 
**Şekil 1: Service Bus, bulut üzerinden uygulamaları bağlamak için çok kiracılı bir hizmet sunar.**

Ad alanı içinde, dört farklı iletişim mekanizmasının bir veya birden fazla örneğini kullanabilirsiniz. Bu mekanizmaların her biri, uygulamaları farklı yöntemlerle bağlar. Seçenekler şunlardır:

- Tek yönlü iletişime izin veren *kuyruklar*. Her kuyruk, alınana kadar gönderilen iletileri depolayan bir ara hizmet olarak görev yapar (bazen *aracı* olarak adlandırılır). Her ileti tek bir alıcı tarafından alınır.
- *Abonelikler* olmak üzere tek konu başlığı kullanan, tek yönlü iletişim sağlayan *Konu Başlıkları*, birden çok aboneliği barındırabilir. Kuyrukta olduğu gibi konu başlığı, aracı gibi davranır ancak her abonelik isteğe bağlı olarak yalnızca belirli kriterlerle eşleşen iletileri almak için filtre kullanabilir.
- Çift yönlü iletişim sunan *geçişler*. Kuyruk ve konu başlıklarının aksine, bir geçiş yürütülen iletileri depolamaz; yani bir aracı değildir. Bunun yerine iletileri yalnızca hedef uygulamaya gönderir.
- Düşük gecikme süresi ve yüksek güvenilirlikle buluta büyük ölçekte olay ve telemetri girişi sağlayan *Event Hubs*.

Bir kuyruk, konu başlığı, geçiş veya Event Hub oluşturduğunuzda, aynı zamanda bunları adlandırırsınız. Ad alanınız ne olursa olsun, bu ad nesne için benzersiz bir tanıtıcı oluşturur. Uygulamalar bu adı Service Bus'a verir ve ardından birbirleriyle iletişim kurmak için bu kuyruğu, konu başlığını, geçişi veya Event Hub hizmetini kullanır. 

Bu nesnelerden herhangi birini kullanmak için Windows uygulaması Windows Communication Foundation'ı (WCF) kullanabilir. Ayrıca, Windows uygulamaları kuyruklar, konu başlıkları ve Event Hubs için Service Bus tanımlı mesajlaşma API'lerini de kullanabilir. Bu nesnelerin Windows uygulaması olmayan uygulamalar tarafından kullanımını kolaylaştırmak için Microsoft Java, Node.js ve diğer dillere yönelik SDK sunar. Ayrıca kuyruklara, konu başlıklarına ve Event Hubs hizmetine HTTP üzerinden REST API'lerini kullanarak da erişebilirsiniz. 

Service Bus hizmeti bulutta (Microsoft'un Azure veri merkezlerinde) çalışıyor olsa da Service Bus hizmetini kullanan uygulamaların herhangi bir yerde çalışabileceğini kavramak önemlidir. Service Bus hizmetini Azure'da çalışan uygulamaları (örneğin, kendi veri merkezinizde çalışan uygulamalar) bağlamak için kullanabilirsiniz . Ayrıca, bu hizmeti Azure veya başka bulut platformunda çalışan bir uygulama ile şirket içi bir uygulamayı veya tablet ve telefonları bağlamak için de kullanabilirsiniz. Ev aletlerini, sensörleri ve diğer cihazları merkezi bir uygulamaya veya başka bir uygulamaya bağlamak bile mümkündür. Service Bus, neredeyse her yerden erişilebilen bulut tabanlı genel bir iletişim mekanizmasıdır. Service Bus hizmetini kullanım şekliniz uygulamanızın gereksinimlerine göre değişir.

## Kuyruklar

İki uygulamayı Service Bus kuyruğu kullanarak bağlamaya karar verdiğinizi varsayalım. Şekil 2'de bu durum gösterilir.

![][2]
 
**Şekil 2: Service Bus kuyrukları tek yönlü, zaman uyumsuz kuyruğa alma işlemi sunar.**

İşlem oldukça basittir: Bir gönderici Service Bus kuyruğuna ileti gönderir ve daha sonra alıcı bu iletiyi alır. Şekil 2'de gösterildiği gibi bir kuyruk yalnızca bir alıcıya sahip olabilir veya birden çok uygulama aynı kuyrukta iletileri okuyabilir. İkinci durumda, her ileti tek bir alıcı tarafından okunur. Çok noktaya yayın yapan bir hizmet için konu başlığı kullanmanız gerekir.

Her ileti iki bölümden oluşur: özellikler kümesi (her biri anahtar/değer çifti olmak üzere) ve ikili ileti gövdesi. Bunların nasıl kullanıldığı, uygulama tarafından gerçekleştirilmeye çalışılan işleme bağlıdır. Örneğin, yakın zamandaki bir satış hakkında bir ileti gönderen uygulama *Seller="Ava"* ve *Amount=10000* özelliklerini içerebilir. İleti gövdesi, satışın imzalı sözleşmesinin taranmış bir görüntüsünü içerebilir veya görüntü mevcut değilse bu kısım boş bırakılır.

Bir alıcı iki farklı şekilde Service Bus kuyruğundaki iletileri okuyabilir. *ReceiveAndDelete* olarak adlandırılan ilk seçenek, kuyruktan iletiyi kaldırır ve anında siler. Bu yöntem basittir ancak alıcı iletiyi işlemeyi tamamlamadan bir çökme gerçekleşirse ileti kaybolur. Kuyruktan kaldırılmış olduğundan başka bir alıcı bu iletiye erişemez. 

*PeekLock* olan ikinci seçenek ise bu soruna çözüm bulmak için tasarlanmıştır. **ReceiveAndDelete** gibi, **PeekLock** yöntemindeki okuma işlemi de iletiyi kuyruktan kaldırır. Ancak iletiyi silmez. Bunun yerine, iletiyi kilitleyerek diğer alıcılar için görünmez yapar ve aşağıdaki olaylardan birinin gerçekleşmesini bekler:

- Alıcı iletiyi başarıyla işlediğinde **Complete** çağrısı yapar ve kuyruk iletiyi siler. 
- Alıcı, iletiyi başarıyla işleyemediğine karar verirse **Abandon** çağrısını yapar. Daha sonra kuyruk iletinin kilidini açar ve iletiyi diğer alıcılar için kullanılabilir hale getirir.
- Ayarlanabilir bir süre içinde alıcı bu yöntemlerin hiçbirini çağırmazsa (varsayılan 60 saniyedir) kuyruk alıcının başarısız olduğunu varsayar. Bu durumda, alıcının **Abandon** çağrısı yaptığını varsayarak hareket eder ve iletiyi diğer alıcılar için kullanılabilir hale getirir.

Burada gerçekleşebilecek şu duruma dikkat edin: Aynı ileti iki kez hatta iki farklı alıcıya teslim edilebilir. Service Bus kuyruklarını kullanan uygulamalar bu duruma karşı hazırlıklı olmalıdır. Yinelenen öğe algılamasını daha kolay hale getirmek için her iletinin benzersiz **MessageID** özelliği vardır. Bu özellik, iletinin kuyruktan kaç kez okunduğuna bakılmaksızın varsayılan olarak sürekli aynıdır. 

Kuyruklar birçok durumda oldukça faydalıdır. Kuyruklar sayesinde aynı anda çalışmayan uygulamaların bile iletişim kurmasına olanak sağlanır. Özellikle toplu işlem ve mobil uygulamalarda olmak üzere bu özellik oldukça kullanışlıdır. Ayrıca, birden çok alıcısı bulunan bir kuyruk otomatik olarak yük dengelemesi sunar. Bu durum, gönderilen iletilerin tüm alıcılara dağıtılmasından kaynaklanır.

## Konu başlıkları

Ne kadar faydalı olsalar da kuyruklar her zaman doğru çözüm değildir. Bazı durumlarda Service Bus konu başlıkları daha faydalıdır. Şekil 3'te bu durum gösterilir.

![][3]
 
**Şekil 3: Abone uygulamanın belirlediği bir filtreye bağlı olarak uygulama, Service Bus konu başlığına gönderilen tüm iletileri veya bazılarını alabilir.**

Konu başlığı, birçok açıdan kuyruğa benzer. Göndericiler, iletileri kuyruğa gönderdikleri gibi aynı şekilde konu başlığına gönderir ve bu iletiler kuyrukta göründükleri gibi görünür. Aradaki büyük fark ise konu başlıklarının alıcı uygulamaların her birinin bir *filtre* belirleyerek kendi aboneliklerini oluşturmalarına olanak sağlamasıdır. Böylece abone yalnızca filtreyle eşleşen iletileri görebilir. Örneğin, Şekil 3'te, bir gönderici ile üç abonesi bulunan bir konu başlığı ve abonelerin her birinin kendi filtrelerine sahip olduğu bir durum gösterilir:

- Abone 1 yalnızca *Seller="Ava"* özelliğini içeren iletileri alır.
- Abone 2 ise *Seller="Ruby"* ve/veya değeri 100.000'den fazla olan *Amount* özelliklerini içeren iletileri alır. Ruby'nin bir satış müdürü olduğunu varsayarsak Ruby kendi satışları haricindeki tüm büyük satışları kimin yaptığına bakmaksızın görmek isteyebilir.
- Abone 3, filtresini *True* olarak ayarlar ve tüm iletileri alır. Örneğin, bu uygulama bir denetim kaydı tutmakla görevlendirilmiştir ve tüm iletileri görmesi gerekir.

Kuyruklarda olduğu gibi, bir konu başlığının aboneleri de iletileri **ReceiveAndDelete** veya **PeekLock** kullanarak okuyabilir. Ancak kuyrukların aksine, konu başlığına gönderilen tek bir ileti birden çok abone tarafından alınabilir. Yaygın şekilde *yayımla ve abone ol* olarak adlandırılan bu yaklaşım, aynı iletilerin birden çok uygulamanın ilgi alanına girmesi durumunda faydalıdır. Her abone, doğru filtreyi tanımlayarak ileti akışının yalnızca görmesi gereken kısmını seçebilir.

## Geçişler

Hem kuyruklar hem de konu başlıkları, bir aracı yoluyla tek yönlü zaman uyumsuz iletişim sağlar. Trafik akışları sadece tek yöndedir ve göndericiler ile alıcılar arasında doğrudan bağlantı yoktur. Peki bunu istemezseniz çözüm nedir? Uygulamalarınızın iletileri hem göndermesi hem de alması gerektiğini ya da gönderici ile alıcılar arasında doğrudan bağlantı istediğinizi ve iletileri depolamak için aracıya ihtiyacınız olmadığını düşünelim. Bunun gibi bir senaryoya uyum sağlamak için Service Bus, Şekil 4'te gösterildiği gibi geçişleri kullanır.

![][4]
 
**Şekil 4: Service Bus geçişi, uygulamalar arasında zaman uyumlu, çift yönlü iletişim sağlar.**

Geçiş kullanımı hakkında akla gelen ilk soru ise şudur: Neden geçiş kullanmalıyım? Kuyruklara ihtiyacım olmasa bile doğrudan etkileşim sağlamak yerine uygulamaların arasındaki iletişimi neden bir bulut hizmeti aracılığıyla sağlamalıyım? Bu soruya verilen cevap ise doğrudan iletişim kurmanın düşündüğünüzden daha zor olmasıdır.

Her ikisi de kurumsal veri merkezlerinde çalışan iki şirket içi uygulama arasında bağlantı kurmak istediğinizi düşünelim. Bu uygulamaların her biri güvenlik duvarının arkasında bulunur ve her veri merkezi de ağ adresi çevirisi (NAT) kullanır. Güvenlik duvarı, birkaçı dışındaki tüm bağlantı noktalarından gelen verileri engeller ve NAT ise uygulamaların çalıştığı makinelerin veri merkezi dışından doğrudan erişebileceğiniz sabit bir IP adresi olmadığını ifade eder. İlave yardım almadan genel İnternet üzerinden bu uygulamaları bağlamak sorun yaratır.

Service Bus geçişi, ihtiyacınız olan yardımı sunar. Bir geçiş aracılığıyla çift yönlü iletişim sağlamak için her uygulama Service Bus içeren bir giden TCP bağlantısı kurar ve bu bağlantıyı açık tutar. İki uygulama arasındaki tüm iletişim bu bağlantılar üzerinden kurulur. Tüm bağlantılar veri merkezi içinden kurulduğundan, güvenlik duvarı her uygulama için gelen trafiğe yeni bağlantı noktaları açmadan izin verir. Ayrıca, her uygulama iletişim boyunca bulutta sabit bir uç noktaya sahip olduğundan, bu çözüm NAT sorununu da ortadan kaldırır.  Uygulamalar, geçiş aracılığıyla veri değişimi yaparak gerçekleşmeleri durumunda iletişimi zorlaştıracak sorunlardan kaçınabilir. 

Service Bus geçişlerini kullanmak için uygulamalar, Windows Communication Foundation'a (WCF) güvenir. Service Bus, Windows uygulamaları için geçişler aracılığıyla etkileşim sağlamayı doğrudan olacak şekilde ayarlayan WCF bağlamalarını sunar. WCF'yi zaten kullanmakta olan uygulamaların genel olarak bu bağlamaların bir tanesini belirtmesi yeterlidir, ardından bir geçiş aracılığıyla birbirleriyle iletişim kurarlar. Öte yandan kuyrukların ve konu başlıklarının aksine, olası durumlarda Windows uygulaması olmayan uygulamalarda geçişlerin kullanılması standart kitaplıklar sağlanmadığından programlama açısından biraz daha fazla çaba sarf edilmesini gerektirir.

Kuyruk ve konu başlıklarının aksine uygulamalar, geçişleri açık bir şekilde oluşturmaz. Bunun yerine, iletileri almak isteyen bir uygulama Service Bus içeren bir TCP bağlantısı kurduğunda otomatik olarak geçiş oluşturulur. Bağlantı kesildiğinde geçiş de silinir. Uygulamanın belirli bir dinleyici tarafından oluşturulan geçişi bulması için Service Bus, uygulamaların ada göre belirli geçişleri bulmalarına olanak sağlayan bir kayıt defteri sunar.

Uygulamalar arasında doğrudan iletişim kurulması gerekiyorsa geçişler doğru çözümdür. Örneğin; check-in kiosk cihazları, mobil cihazlar ve diğer bilgisayarlardan erişilebilmesi gereken ve bir şirket içi veri merkezinde çalışan hava yolu rezervasyon sistemini ele alalım. Bu sistemlerin tümünde çalışan uygulamalar, nerede çalışıyor olurlarsa olsunlar iletişim kurmak için buluttaki Service Bus geçişlerine güvenebilirler.

## Event Hubs

Event Hubs, saniye başına milyonlarca olayı işleyen ileri düzeyde ölçeklenebilir bir alım sistemidir. Bu sistem, uygulamanızın bağlı cihazlarınız ve uygulamalarınız tarafından üretilen oldukça büyük miktardaki veriyi işlemesine ve analiz etmesine olanak sağlar. Örneğin, bir araba filosundan canlı motor performansı verilerini toplamak için Event Hub hizmetini kullanabilirsiniz. Veriler Event Hubs hizmetinde toplandığında, herhangi bir gerçek zamanlı analitik sağlayıcısı veya depolama kümesi kullanarak bu verileri dönüştürebilir veya depolayabilirsiniz. Event Hubs hakkında daha fazla bilgi için bkz. [Event Hubs hizmetine genel bakış](../event-hubs/event-hubs-overview.md)

## Özet

Uygulamalar arasında bağlantı kurma her zaman eksiksiz çözüm derlemelerinin bir parçası olmuştur. Bunun yanı sıra, uygulamaların ve hizmetlerin birbirleriyle iletişim kurmasını gerektiren senaryoların sayısı daha fazla uygulamanın ve cihazın İnternet'e bağlanmasıyla artmaktadır. Bu gibi durumlarda istenilen başarıya ulaşmak için kuyrukların, konu başlıklarının, geçişlerin ve Event Hubs hizmetinin kullanılmasına yönelik bulut tabanlı teknolojiler sağlayan Service Bus, bu temel işlevin uygulanmasını kolaylaştırmayı ve daha geniş kapsamda kullanılmasını amaçlar.

## Sonraki adımlar

Artık Azure Service Bus hizmeti ile ilgili temel bilgileri edindiğinize göre, daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin.

- [Service Bus kuyruklarını](service-bus-dotnet-how-to-use-queues.md) kullanma
- [Service Bus konu başlıklarını](service-bus-dotnet-how-to-use-topics-subscriptions.md) kullanma
- [Service Bus geçişini](service-bus-dotnet-how-to-use-relay.md) kullanma
- [Service Bus örnekleri](service-bus-samples.md)

[1]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_01_architecture.png
[2]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_02_queues.png
[3]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_03_topicsandsubscriptions.png
[4]: ./media/service-bus-fundamentals-hybrid-solutions/SvcBus_04_relay.png



<!---HONumber=Jun16_HO2-->


