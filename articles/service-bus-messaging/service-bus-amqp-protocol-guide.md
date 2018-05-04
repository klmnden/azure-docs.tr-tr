---
title: AMQP 1.0 Azure Service Bus ve Event Hubs Protokolü Kılavuzu | Microsoft Docs
description: İfadeler ve Azure Service Bus ve Event Hubs AMQP 1.0 açıklamasını Protokolü Kılavuzu
services: service-bus-messaging,event-hubs
documentationcenter: .net
author: clemensv
manager: timlt
editor: ''
ms.assetid: d2d3d540-8760-426a-ad10-d5128ce0ae24
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/30/2018
ms.author: clemensv
ms.openlocfilehash: e124ea3f932a81634191785e7ee69c2492cb32fa
ms.sourcegitcommit: 6e43006c88d5e1b9461e65a73b8888340077e8a2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2018
---
# <a name="amqp-10-in-azure-service-bus-and-event-hubs-protocol-guide"></a>Azure Service Bus ve Event Hubs Protokolü Kılavuzu'nda AMQP 1.0

Gelişmiş Message Queuing protokolü 1.0 zaman uyumsuz olarak, güvenli ve güvenilir bir şekilde iletilerini iki taraf arasında aktarma için standartlaştırılmış bir çerçeveleme ve Aktarım Protokolü ' dir. Azure Service Bus Mesajlaşma hizmeti ve Azure Event Hubs birincil protokolüdür. Her iki hizmet de HTTPS destekler. Ayrıca desteklenen özel SBMP Protokolü lehinde AMQP aşamalı olarak Sonlandırılmakta.

AMQP 1.0 ara yazılım satıcıları, Microsoft ve Red Hat gibi JP Morgan finansal hizmetler endüstri temsil eden Chase gibi birçok Mesajlaşma ara yazılım kullanıcılarla birlikte duruma geniş endüstri işbirliği sonucudur. AMQP protokolünü ve uzantı özellikleriyle ilgili teknik Standartlaştırma Forumu OASIS olduğu ve ISO/IEC 19494 olarak uluslararası bir standart olarak resmi onay kazanmıştır.

## <a name="goals"></a>Hedefleri

Bu makalede kısaca OASIS AMQP teknik komitesi şu anda sonuçlandırılmış taslak uzantısı belirtimleri, küçük bir kümesini birlikte belirtimi Mesajlaşma AMQP 1.0 çekirdek kavramlarını özetler ve nasıl Azure Service Bus uygular ve bu belirtimlere bağlı derlemeler açıklanmaktadır.

Tüm mevcut AMQP 1.0 istemcisi yığını, Azure hizmet veri yolu AMQP 1.0 aracılığıyla etkileşim kurabilmesi için herhangi bir platformda kullanarak herhangi bir geliştirici için belirtilir.

Apache Proton veya AMQP.NET Lite gibi ortak genel amaçlı AMQP 1.0 yığınları zaten tüm çekirdek AMQP 1.0 protokollerini uygulayın. Bu temel hareketleri bazen bir üst düzey API'si ile sarılır; Apache Proton bile iki, kesinlik temelli Messenger API ve reaktif rektör API sunar.

Aşağıdaki tartışmada AMQP bağlantıları, oturumları ve bağlantıları yönetimini ve çerçeve aktarımları ve akış denetimi işlenmesini ilgili yığın (gibi Apache Protonun durgun-C) tarafından işlenir ve uygulama geliştiricileri özel kadar varsa işlem yapmanız gerekli değil varsayıyoruz. Bağlamalarında birkaç API temelleri yeteneği gibi bağlanmak ve çeşit oluşturmak için varlığını varsayıyoruz *gönderen* ve *alıcı* soyutlama nesneleri, daha sonra sahip bazı şeklini `send()` ve `receive()` işlemleri, sırasıyla.

Azure Service Bus, gelişmiş özelliklerini ileti gözatma veya oturumlar, yönetimi gibi ele alırken bu özellikleri AMQP terimleriyle aynı zamanda bu varsayılan API soyutlama üstünde katmanlı sahte bir uygulama olarak açıklanmıştır.

## <a name="what-is-amqp"></a>AMQP nedir?

AMQP çerçeveleme ve aktarım protokolüdür. Çerçeveleme anlamına gelir, bir ağ bağlantısı her iki yönde akmasını ikili veri akışları için yapısı sağlar. Kabul edilebilir açıklıkta ayrı blokları adlandırılan bir veri yapısı sağlar *çerçeveleri*, bağlı taraflar arasında değiştirilmek üzere. Aktarma Özellikleri iletişim kuran her iki taraf çerçeveler zaman aktarılan ve ne zaman aktarımları tam değerlendirilmesi hakkında paylaşılan bir anlayış kurup emin olun.

Hala birkaç ileti aracıları tarafından kullanılmakta olan AMQP çalışma grubu tarafından üretilen süresi dolan taslak önceki sürümlerden farklı olarak, ileti Aracısı ya da herhangi bir ileti Aracısı içindeki varlıklar için belirli topolojisi varlığını çalışma grubunun son ve standartlaştırılmış AMQP 1.0 protokolü belgenizdeki değil.

Protokolü gibi Azure Service Bus kuyrukları ve varlıkları, Yayımla/abone ol destekleyen ileti aracıları ile etkileşim için simetrik eşler arası iletişim için kullanılabilir. Bu ayrıca Mesajlaşma altyapısı ile etkileşim için Azure Event Hubs ile olduğu gibi etkileşim desenleri normal sıralarından farklı olduğu kullanılabilir. Bir Event Hub olayları için gönderildiğinde bir kuyruk gibi davranır, ancak olayları ondan okunduğunda daha seri bağlantılı depolama hizmeti gibi davranır; Ayrıca bir bant sürücüsü biraz benzer. İstemci uzaklığı kullanılabilir veri akışa seçer ve ardından bu uzaklık tüm olayları için en son kullanılabilir sunulur.

AMQP 1.0 protokolü, daha fazla yeteneklerini geliştirmek belirtimleri etkinleştirme Genişletilebilir olmak üzere tasarlanmıştır. Bu belgede açıklanan üç uzantısı belirtimleri bu gösterilmektedir. Burada yerel AMQP TCP bağlantı noktalarını yapılandırma belirlenemeyebilir varolan HTTPS/WebSockets altyapı üzerinden iletişim için bir bağlama belirtimi AMQP WebSockets üzerinden Katman nasıl tanımlar. Mesajlaşma altyapısı yönetim amacıyla veya gelişmiş işlevselliği sağlamak için bir istek/yanıt şekilde etkileşimde için gerekli temel etkileşim temelleri AMQP yönetim belirtimi tanımlar. Federasyon yetkilendirme Modeli tümleştirmesi için AMQP talep tabanlı güvenlik belirtimi ilişkilendirmek ve bağlantıları ile ilişkili yetkilendirme belirteçleri yenilemek nasıl tanımlar.

## <a name="basic-amqp-scenarios"></a>Temel AMQP senaryoları

Bu bölümde AMQP 1.0 bağlantıları, oturumları ve bağlantıları oluşturma ve hizmet veri yolu varlıklarını kuyruklar, konu başlıkları ve abonelikler gibi gelen ve giden iletilerini aktarma içeren Azure Service Bus ile temel kullanımını açıklar.

AMQP nasıl çalıştığı hakkında bilgi edinmek için en yetkili kaynağı için AMQP 1.0 belirtimi olsa da, belirtimi tam olarak Uygulama Kılavuzu ve protokolü öğretmek değildir yazıldı. Bu bölümde, hizmet veri yolu AMQP 1.0 nasıl kullandığını tanımlamak için gerektiği şekilde kadar terminolojisi Tanıtımı odaklanır. AMQP daha kapsamlı bir giriş, yanı sıra daha geniş bir tartışma AMQP 1.0, gözden geçirebilirsiniz [bu video indirmelere][this video course].

### <a name="connections-and-sessions"></a>Bağlantıları ve oturumlar

AMQP iletişim kuran programlar çağırır *kapsayıcıları*; bu içeren *düğümleri*, bu kapsayıcı içinde iletişim kuran varlıkları olduğu. Bir kuyruk böyle bir düğüm olabilir. Tek bir bağlantı düğümler arasında birçok iletişim yolları için kullanılabilmesi için AMQP çoğullama için sağlar; Örneğin, bir uygulamanın istemci aynı anda bir kuyruktan alabilir ve başka bir sırayla aynı ağ bağlantısı üzerinden gönder.

![][1]

Ağ bağlantısı, böylece kapsayıcısında sabitlenmiştir. Bir giden TCP yuva bağlantısı için bir kapsayıcı dinler ve gelen TCP bağlantılarını kabul alıcı rolünde yapmadan istemci rolü kapsayıcısında tarafından başlatılır. Bağlantı el sıkışması bildirme veya Aktarım Katmanı Güvenliği (TLS/SSL) kullanımını ve bir kimlik doğrulama/yetkilendirme el sıkışma SASL üzerinde temel bağlantı kapsamındaki anlaşması Protokolü sürüm anlaşması içerir.

Azure hizmet veri yolu her zaman TLS kullanımını gerektirir. TCP bağlantısı AMQP Protokolü anlaşması girmeden önce TLS ile ilk yayılan aslına 5671, TCP bağlantı noktası üzerinden bağlantılarını destekler ve ayrıca sunucu hemen bağlantının zorunlu bir yükseltme için TLS AMQP belirlenen modelini kullanarak teklif aslına 5672 TCP bağlantı noktası üzerinden bağlantılarını destekler. AMQP WebSockets bağlama AMQP 5671 bağlantı eşdeğer olan TCP bağlantı noktası 443 üzerinden bir tünel oluşturur.

TLS ve bağlantı ayarladıktan sonra Service Bus iki SASL mekanizması seçenekleri sunar:

* SASL DÜZ genellikle, kullanıcı adı ve parola kimlik bilgileri bir sunucuya geçirmek için kullanılır. Hizmet veri yolu adlandırılmış ancak hesapları yok [paylaşılan erişim güvenlik kuralları](service-bus-sas.md), hakları confer ve bir anahtar ile ilişkili. Kullanıcı adı ve anahtar kullanılan bir kural adı (metin Base64 ile kodlanmış gibi) parola olarak kullanılır. Seçilen kuralı ile ilişkilendirilmiş hakları bağlantısından işlemlerini yönetir.
* SASL anonim istemci daha sonra açıklanan talep tabanlı güvenlik (CBS) modeli kullanmak istediğinde SASL yetkilendirme atlayarak için kullanılır. Bu seçenek ile bir istemci bağlantısı anonim olarak kısa hangi sırasında istemci yalnızca CBS uç noktasıyla etkileşim kurabilir ve CBS el sıkışma tamamlamalısınız bir süre kurulabilir.

Taşıma bağlantısı kurulduktan sonra her kapsayıcı işlemeye hazır olduğunuz en büyük çerçeve boyutunu bildirme ve bağlantıda etkinlik yoksa boşta kalma zaman aşımından sonra bunlar tek taraflı bağlantısını.

Ayrıca kaç eşzamanlı kanalları desteklenen bildirin. Bir kanal bağlantısı üzerinde tek yönlü giden, sanal aktarımı yoludur. Bir oturum kapsayıcıların bir çift yönlü iletişim yolu oluşturacak şekilde birbirine bağlı bir kanaldan alır.

Oturumları pencere tabanlı akış denetimi modeli vardır; bir oturum oluşturulduğunda, kaç tane çerçeve içine kabul etmeye hazır taraflara bildirir onun penceresi alırsınız. Tarafların exchange çerçeve penceresi ve aktarımları penceresi dolu olduğunda ve pencere sıfırlama veya genişletilmiş kullanarak ulaşana kadar Durdur aktarılan çerçeve dolgu olarak *akış performative* (*performative* iki taraf arasında alınıp protokol düzeyi hareketleri için AMQP terimdir).

Bu pencere tabanlı TCP kavramı Windows tabanlı akış denetimi ancak yuva içinde oturum düzeyinde için kabaca modelidir. Yüksek öncelikli trafiği daraltılmış normal trafiği gibi Otoyol express Şerit üzerinde rushed böylece birden çok eşzamanlı oturumlar için izin verme kavramı protokolün bulunmaktadır.

Azure hizmet veri yolu, her bağlantı için şu anda tam olarak bir oturum kullanır. Hizmet veri yolu en fazla çerçeve-Service Bus standart ve Event Hubs için 262.144 bayt (256 K bayt) boyutudur. Service Bus Premium için 1.048.576 (1 MB) olur. Hizmet veri yolu herhangi belirli oturum düzeyi azaltma windows getirmez, ancak pencere düzenli olarak bağlantı düzeyi akış denetimi bir parçası olarak sıfırlar (bkz [sonraki bölümde](#links)).

Bağlantılar, Kanallar ve oturumları kısa ömürlü. Bağlantılar, temel alınan bağlantı daraltır, TLS tüneli, SASL yetkilendirme bağlamı ve oturumları kurulmaları gerekir.

### <a name="links"></a>Bağlantılar

AMQP iletileri bağlantıları üzerinden aktarılır. Bir yönde aktarma iletileri sağlayan bir oturumu üzerinden oluşturulan bir iletişim yolu bağlantıdır; Aktarım durumu anlaşma bağlantı ve bağlı taraflar arasında çift yönlü sona erer.

![][2]

Bağlantılar, herhangi bir zamanda ve AMQP HTTP ve başlatma aktarımları ve aktarım yolu yuva bağlantı oluşturma taraf özel bir ayrıcalık olduğu MQTT dahil birçok diğer protokoller farklı yapar bir varolan oturumu üzerinden ya da kapsayıcı tarafından oluşturulabilir.

Bir bağlantı kabul etmek için ters kapsayıcı bağlantıyı başlatan kapsayıcı ister ve rol göndereni veya alıcısı olarak seçer. Bu nedenle, kapsayıcı tek yönlü oluşturma başlatabilir ya da çift yönlü iletişim yolları, ikinci ile bağlantılar çiftleri olarak modellenir.

Bağlantılar adlı ve düğümleri ile ilişkilendirilmiş. ' Den itibaren belirtildiği gibi düğümleri bir kapsayıcı içinde iletişim kuran varlıklardır.

Hizmet veri yolu, bir düğüm doğrudan bir kuyruk, konu, bir abonelik veya bir kuyruk veya abonelik sahipsiz sırasına eşdeğerdir. AMQP içinde kullanılan düğüm adı bu nedenle göreli Service Bus ad alanı içinde varlıkla adıdır. Bir kuyruk adlandırılmışsa **Sıram**, ayrıca kendi AMQP düğüm adı olmasıdır. Bir konu aboneliği "abonelikleri" kaynak koleksiyonu ve bu nedenle, bir abonelik sıralanmış tarafından HTTP API kuralı izleyen **alt** veya bir konu **mytopic** AMQP düğüm adına sahip **abonelikleri/mytopic/alt**.

Bağlanan istemcinin de bağlantılar oluşturmak için bir yerel düğüm adı kullanmak için gereklidir; Service Bus bu düğüm adları hakkında Düzenleyici değil ve bunları yorumlar değil. AMQP 1.0 istemcisi yığınları genellikle bir düzeni güvence altına almak için bu kısa ömürlü düğüm adları istemci kapsamında benzersiz olan kullanın.

### <a name="transfers"></a>Aktarımları

Bağlantı kurulduktan sonra o bağlantı üzerinden iletileri aktarılabilir. AMQP içinde bir aktarımı ile bir açık Protokolü hareketi yürütülür ( *aktarımı* performative), bir iletiyi gönderenden alıcıya bir bağlantı üzerinden taşır. Bir aktarım her iki taraf bu aktarım sonucunu paylaşılan bir anlayış kurduktan anlamına gelir, bu "kapatıldığında", tamamlanmış olur.

![][3]

En basit durumda, istemci sonucu ilgilenen değil ve alıcı işlemin sonucuyla ilgili herhangi bir geri bildirim sağlamıyor anlamına gelen "önceden kapatılmış," iletileri göndermek gönderen seçebilirsiniz. Bu mod, hizmet veri yolu AMQP protokol düzeyinde tarafından desteklenen ancak istemci API hiçbirinde gösterilmeyen.

Normal iletileri gönderilir kapatılmamış ve alıcı sonra kabul veya reddetme kullanarak gösterir durumdur *değerlendirme* performative. Alıcı iletiyi herhangi bir nedenle kabul edilemiyor ve reddetme ileti AMQP tarafından tanımlanan bir hata yapısıdır nedenine ilişkin bilgi içerir reddetme oluşur. Service Bus içinde iç hatalar nedeniyle iletileri reddedilirse hizmeti destek istekleri dosyalama, destek personelinin tanılama ipuçları sunmak için kullanılabilecek yapıyı içindeki ek bilgi döndürür. Daha sonra hatalar hakkında daha fazla bilgi edineceksiniz.

Özel reddetme biçimidir *yayımlanan* alıcı hiçbir teknik itiraz aktarımı ancak aktarımı şekilde de ayrıca faiz yok olduğunu gösteren durum. İleti işlenirken oluşan iş gerçekleştiremezsiniz var olduğundan, bu durumda, örneğin, bir ileti bir Service Bus istemciye teslim edilir ve "iletisi iptal etmek" istemci seçer; ileti teslimi hatalı değil. Bu durumda çeşididir *değiştiren* yapılacak değişiklikler yayımlandıktan olarak veren durumu. Bu durumda, şu anda Service Bus tarafından kullanılmaz.

Başka bir değerlendirme AMQP 1.0 belirtimi tanımlar adlı durumu *alınan*, bağlantı kurtarma işlemek için özellikle yardımcı olur. Bir bağlantı ve herhangi bir yeni bağlantı ve oturumu, oturum ve önceki bağlantı zaman kayboldu üstünde teslimler bekleyen durumunu reconstituting bağlantı kurtarma sağlar.

Hizmet veri yolu bağlantı kurtarma desteklemiyor; istemci için Service Bus kapatılmamış iletisine ile bağlantısı kesilirse bekleyen Aktarım, bu ileti aktarma kaybolur ve istemci yeniden bağlanma, bağlantıyı yeniden kurmak ve aktarımı yeniden denemeniz gerekir.

Bu nedenle, hizmet veri yolu ve Event Hubs "en az bir kez" nerede gönderen iletisinin depolanır ve kabul olabilirsiniz, ancak "tam bir kez" bağlantıyı kurtarmak ve ileti aktarma yinelenmesini önlemek için teslim durumu anlaşmak üzere devam etmek için sistem burada denenecek AMQP düzeyinde aktarımları desteklemez aktarımlarını destekler.

Olası yinelenen gönderir dengelemek için Service Bus kuyrukları ve konularından isteğe bağlı bir özellik olarak yinelenen algılama destekler. Yinelenen algılama gelen tüm iletilerin iletisi kimlikleri bir kullanıcı tanımlı bir zaman penceresi sırasında kaydeder ve sonra o aynı penceresi sırasında aynı iletisi kimlikleri ile gönderilen tüm iletiler sessiz bir şekilde bırakır.

### <a name="flow-control"></a>Akış denetimi

Daha önce ele alınan oturum düzeyi akış denetimi modeli yanı sıra, her bağlantı kendi akış denetimi modeli vardır. Oturum düzeyi akış denetimi en çok çerçeve bağlantı düzeyi akış denetimi bir bağlantıdan işlemek istediği kaç iletileri sorumlu uygulama koyar sonra ve ne zaman işlemek zorunda kalmaktan kapsayıcı korur.

![][4]

Göndereni yeterli olan bir bağlantıda aktarımları yalnızca ortaya çıkar *bağlantı kredi*. Bağlantı alacak olan kullanarak alıcı tarafından sayacın *akış* performative, bağlantı kapsamlı. Gönderen bağlantısı kredi atandığında, bu kredi iletileri sunarak kullanmayı dener. Her ileti teslim azaltır kalan bağlantı 1 ile alacak. Bağlantı alacak kullanıldığı zaman teslimler durdurun.

Hizmet veri yolu alıcı rolde olduğunda, böylece iletileri hemen gönderilebilir, anında gönderen geniş bağlantı kredi ile sağlar. Hizmet veri yolu bağlantı alacak kullanıldığı şekilde bazen gönderir bir *akış* bağlantı kredi bakiyesi güncelleştirmek için gönderene performative.

Gönderen rolü'nde, Service Bus tüm bekleyen bağlantı kredi kullanılacak iletileri gönderir.

"Alma" çağrısı API düzeyinde çevirir bir *akış* performative Service Bus istemci tarafından ve Service Bus gönderilen kullanır, kredi ilk kullanılabilir, kilitsiz iletiyi kuyruktan alma kilitleyerek ve onu aktarılıyor. Teslimat için kullanıma hazır bir ileti ise tüm bekleyen kredi herhangi bir bağlantıyı tarafından ile belirli varlık varış sırayla kayıtlı olarak kalır, ve iletiler kilitli ve tüm bekleyen kredi kullanmak, çıktıklarında aktarılan kurdu.

Aktarım terminal durumlarından birine kapatıldığında bir ileti kilidi serbest *kabul*, *reddetti*, veya *serbest*. Terminal durumuna olduğunda iletinin hizmet yolundan kaldırılıyor *kabul*. Hizmet veri yolundaki kalır ve aktarım diğer durumlardan ulaştığında sonraki alıcıya teslim edilir. Yinelenen redleri ya da sürümlerden nedeniyle varlık için izin verilen maksimum teslimat sayısı ulaştığında Service Bus iletiyi varlığın sahipsiz sıraya otomatik olarak taşır.

Service Bus API'lerine böyle bir seçenek bugün doğrudan gösterme olsa da, bir alt düzey AMQP protokolünü istemci çok sayıda bağlantı krediler vererek "stili itme" modeline kredi her alma isteği için bir ölçü veren, "çekme stili" etkileşim açın ve başka etkileşimi kullanılabilir olduklarında iletileri almak için bağlantı kredi modelini kullanabilirsiniz. Anında iletme aracılığıyla desteklenir [MessagingFactory.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagingfactory#Microsoft_ServiceBus_Messaging_MessagingFactory_PrefetchCount) veya [MessageReceiver.PrefetchCount](/dotnet/api/microsoft.servicebus.messaging.messagereceiver#Microsoft_ServiceBus_Messaging_MessageReceiver_PrefetchCount) özelliğini ayarlar. Sıfır olmayan olduklarında AMQP istemci bağlantı alacak olarak kullanır.

Bu bağlamda ileti iletinin zaman kablo put değil varlıktan alındığında kilit varlığı içinde iletideki sona erme saati başlayacağını anlamak önemlidir. İstemci bağlantı kredi vererek iletileri almaya hazır olma durumunu gösteren olduğunda, bu nedenle iletileri ağ üzerinde etkin olarak çekme ve bunları işlemek için hazır olması beklenir. Aksi takdirde iletisi bile teslim edilmeden önce iletiyi kilit dolmuş olabilir. Bağlantı kredi akış denetimi kullanarak doğrudan alıcıya gönderilen kullanılabilir iletiler uğraşmanız hemen hazırlık yansıtmalıdır.

Özet olarak, aşağıdaki bölümlerde farklı API etkileşimleri sırasında performative akış şematik bir genel bakış sağlar. Her bölümde farklı bir mantıksal işlem açıklanmaktadır. Bu etkileşimler bazıları "yavaş bunlar yalnızca gerekli olduğunda gerçekleştirilebilir anlamına gelir," olabilir. İlk ileti gönderilen ya da istenen kadar bir ileti gönderen oluşturma ağ etkileşim neden.

Aşağıdaki tabloda oklar performative akış yönü gösterir.

#### <a name="create-message-receiver"></a>İleti alıcı oluşturma

| İstemci | Service Bus |
| --- | --- |
| --> () ekleme<br/>adı = {bağlantı adı}<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Rol =**alıcı**,<br/>Kaynak = {varlık adı}<br/>Hedef = {istemci bağlantı kimliği}<br/>) |İstemci varlık alıcısı olarak ekler. |
| Hizmet veri yolu yanıt bağlantıyı kendi sonuna ekleme |<--ekleme ()<br/>adı = {bağlantı adı}<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Rol =**gönderen**,<br/>Kaynak = {varlık adı}<br/>Hedef = {istemci bağlantı kimliği}<br/>) |

#### <a name="create-message-sender"></a>İleti göndereni oluşturma

| İstemci | Service Bus |
| --- | --- |
| --> () ekleme<br/>adı = {bağlantı adı}<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Rol =**gönderen**,<br/>Kaynak = {istemci bağlantı kimliği}<br/>Hedef = {varlık adı}<br/>) |Eylem yok |
| Eylem yok |<--ekleme ()<br/>adı = {bağlantı adı}<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Rol =**alıcı**,<br/>Kaynak = {istemci bağlantı kimliği}<br/>Hedef = {varlık adı}<br/>) |

#### <a name="create-message-sender-error"></a>İletiyi gönderen (hata) oluşturma

| İstemci | Service Bus |
| --- | --- |
| --> () ekleme<br/>adı = {bağlantı adı}<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Rol =**gönderen**,<br/>Kaynak = {istemci bağlantı kimliği}<br/>Hedef = {varlık adı}<br/>) |Eylem yok |
| Eylem yok |<--ekleme ()<br/>adı = {bağlantı adı}<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Rol =**alıcı**,<br/>Kaynak = null,<br/>Hedef = null<br/>)<br/><br/><--detach ()<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Kapalı =**doğru**,<br/>hata = {hata bilgisini}<br/>) |

#### <a name="close-message-receiversender"></a>İletiyi Kapat alıcı/gönderen

| İstemci | Service Bus |
| --- | --- |
| --> detach ()<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Kapalı =**true**<br/>) |Eylem yok |
| Eylem yok |<--detach ()<br/>tanıtıcı {sayısal tanıtıcı} =<br/>Kapalı =**true**<br/>) |

#### <a name="send-success"></a>Gönder (başarılı)

| İstemci | Service Bus |
| --- | --- |
| Aktarım (--><br/>teslim kimliği = {sayısal tanıtıcı}<br/>teslim etiketi {ikili tanıtıcı} =<br/>Kapatılan =**false**,, daha =**yanlış**,<br/>Durum =**null**,<br/>sürdürme =**false**<br/>) |Eylem yok |
| Eylem yok |<--değerlendirme ()<br/>Rol alıcı =<br/>ilk = {teslim kimliği}<br/>Son = {teslim kimliği}<br/>Kapatılan =**doğru**,<br/>Durum =**kabul edildi**<br/>) |

#### <a name="send-error"></a>Gönder (hata)

| İstemci | Service Bus |
| --- | --- |
| Aktarım (--><br/>teslim kimliği = {sayısal tanıtıcı}<br/>teslim etiketi {ikili tanıtıcı} =<br/>Kapatılan =**false**,, daha =**yanlış**,<br/>Durum =**null**,<br/>sürdürme =**false**<br/>) |Eylem yok |
| Eylem yok |<--değerlendirme ()<br/>Rol alıcı =<br/>ilk = {teslim kimliği}<br/>Son = {teslim kimliği}<br/>Kapatılan =**doğru**,<br/>Durum =**reddetti**()<br/>hata = {hata bilgisini}<br/>)<br/>) |

#### <a name="receive"></a>Al

| İstemci | Service Bus |
| --- | --- |
| Akış (--><br/>bağlantı kredi = 1<br/>) |Eylem yok |
| Eylem yok |< Aktarım ()<br/>teslim kimliği = {sayısal tanıtıcı}<br/>teslim etiketi {ikili tanıtıcı} =<br/>Kapatılan =**yanlış**,<br/>Daha fazla =**yanlış**,<br/>Durum =**null**,<br/>sürdürme =**false**<br/>) |
| Değerlendirme (--><br/>Rol =**alıcı**,<br/>ilk = {teslim kimliği}<br/>Son = {teslim kimliği}<br/>Kapatılan =**doğru**,<br/>Durum =**kabul edildi**<br/>) |Eylem yok |

#### <a name="multi-message-receive"></a>Çok iletisi alıyorsunuz

| İstemci | Service Bus |
| --- | --- |
| Akış (--><br/>bağlantı kredi = 3<br/>) |Eylem yok |
| Eylem yok |< Aktarım ()<br/>teslim kimliği = {sayısal tanıtıcı}<br/>teslim etiketi {ikili tanıtıcı} =<br/>Kapatılan =**yanlış**,<br/>Daha fazla =**yanlış**,<br/>Durum =**null**,<br/>sürdürme =**false**<br/>) |
| Eylem yok |< Aktarım ()<br/>teslim kimliği = {sayısal tanıtıcı + 1}<br/>teslim etiketi {ikili tanıtıcı} =<br/>Kapatılan =**yanlış**,<br/>Daha fazla =**yanlış**,<br/>Durum =**null**,<br/>sürdürme =**false**<br/>) |
| Eylem yok |< Aktarım ()<br/>teslim kimliği = {sayısal tanıtıcı + 2}<br/>teslim etiketi {ikili tanıtıcı} =<br/>Kapatılan =**yanlış**,<br/>Daha fazla =**yanlış**,<br/>Durum =**null**,<br/>sürdürme =**false**<br/>) |
| Değerlendirme (--><br/>Rol alıcı =<br/>ilk = {teslim kimliği}<br/>Son = {teslim kimliği + 2}<br/>Kapatılan =**doğru**,<br/>Durum =**kabul edildi**<br/>) |Eylem yok |

### <a name="messages"></a>İletiler

Aşağıdaki bölümlerde, hangi özellikler standart AMQP ileti bölümlerden Service Bus tarafından kullanılır ve bunların hizmet veri yolu API'sini kümesine nasıl eşleneceğine açıklanmaktadır.

AMQP için 's tanımlar uygulaması gereken herhangi bir özellik eşlenmelidir `application-properties` eşleme.

#### <a name="header"></a>üst bilgi

| Alan adı | Kullanım | API adı |
| --- | --- | --- |
| dayanıklı |- |- |
| öncelik |- |- |
| ttl |Bu iletinin yaşam süresi |[TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive) |
| ilk alıcı |- |- |
| Teslimat sayısı |- |[DeliveryCount](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_DeliveryCount) |

#### <a name="properties"></a>properties

| Alan adı | Kullanım | API adı |
| --- | --- | --- |
| ileti kimliği |Bu ileti uygulama tanımlı, serbest biçimli tanımlayıcısı. Yinelenen algılama için kullanılır. |[MessageID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_MessageId) |
| Kullanıcı Kimliği |Service Bus tarafından yorumlanan değil, uygulama tanımlı kullanıcı tanımlayıcısı. |Hizmet veri yolu API'si üzerinden erişilebilir değil. |
| - |Service Bus tarafından yorumlanan değil, uygulama tanımlı hedef tanımlayıcısı. |[Alıcı](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_To) |
| Konu |Service Bus tarafından yorumlanan değil, uygulama tanımlı ileti amacı tanımlayıcısı. |[Etiket](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) |
| Yanıtla |Uygulama tanımlı yanıt yolu göstergesi, Service Bus tarafından yorumlanan değil. |[ReplyTo](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyTo) |
| Bağıntı Kimliği |Service Bus tarafından yorumlanan değil, uygulama tanımlı bağıntı tanımlayıcısı. |[correlationıd değeri](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_CorrelationId) |
| içerik türü |İçerik türü, hizmet veri yolu tarafından yorumlanan değil gövdesi için uygulama tanımlı göstergesi. |[ContentType](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ContentType) |
| İçerik kodlama |Uygulama tanımlı içerik kodlama göstergesi için Service Bus tarafından yorumlanan değil gövdesi. |Hizmet veri yolu API'si üzerinden erişilebilir değil. |
| mutlak bitiş zamanı |Hangi mutlak anlık ileti süresi sırasında bildirir. Giriş (TTL gözlenen başlığı), göz ardı çıktıyı yetkili. |[ExpiresAtUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ExpiresAtUtc) |
| oluşturma zamanı |Bildirir aynı zamanda ileti oluşturuldu. Service Bus tarafından kullanılmıyor |Hizmet veri yolu API'si üzerinden erişilebilir değil. |
| Grup Kimliği |Uygulama tanımlı ilgili bir dizi ileti tanımlayıcısı. Hizmet veri yolu oturumları için kullanılır. |[SessionID](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_SessionId) |
| Grup Sırası |İleti bir oturum içine göreli sıra numarasını tanımlayan sayacı. Service Bus tarafından yoksayılır. |Hizmet veri yolu API'si üzerinden erişilebilir değil. |
| yanıt için Grup Kimliği |- |[ReplyToSessionId](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage#Microsoft_ServiceBus_Messaging_BrokeredMessage_ReplyToSessionId) |

#### <a name="message-annotations"></a>İleti ek açıklamaları

AMQP ileti özellikleri bir parçası olmayan ve boyunca olarak geçirilir birkaç diğer service bus ileti özellikleri vardır `MessageAnnotations` ileti üzerinde.

| Ek açıklama harita anahtar | Kullanım | API adı |
| --- | --- | --- |
| x-opt-zamanlanmış-sıraya alma-time | Bildirir aynı zamanda varlık üzerinde iletisi görüntülenmelidir |[ScheduledEnqueueTime](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.scheduledenqueuetimeutc?view=azure-dotnet) |
| x opt bölüm anahtarı | Hangi bölümünü belirleyen uygulama tanımlı anahtar içinde ileti güden. | [PartitionKey](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.partitionkey?view=azure-dotnet) |
| x-opt-aracılığıyla-bölüm-anahtarı | Bir aktarım kuyruk aracılığıyla iletileri göndermek için kullanılacak bir işlem olduğunda, uygulama tanımlı bölüm anahtarı değeri. | [ViaPartitionKey](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.viapartitionkey?view=azure-dotnet) |
| x opt sıraya alınan zamanı | Hizmet tanımlı UTC saati nedeniyle ileti gerçek süresini temsil eden. Giriş, yoksayıldı. | [EnqueuedTimeUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedtimeutc?view=azure-dotnet) |
| x opt sıra numarası | İleti atanan hizmet tanımlı benzersiz sayı. | [SequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.sequencenumber?view=azure-dotnet) |
| x opt uzaklığı | İleti sıraya alınan hizmet tanımlı sıra sayısı. | [EnqueuedSequenceNumber](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.enqueuedsequencenumber?view=azure-dotnet) |
| x-opt-kilitli-kadar | Hizmet tanımlı. Tarih ve saat kadar sıra/abonelik ileti kilitlenmiş olabilir. | [LockedUntilUtc](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.lockeduntilutc?view=azure-dotnet) |
| x opt sahipsiz kaynak | Hizmet tanımlı. İleti sahipsiz sıradan kaynağı özgün iletisi alırsanız. | [DeadLetterSource](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.deadlettersource?view=azure-dotnet) |

### <a name="transaction-capability"></a>İşlem özelliği

Bir işlem, iki veya daha fazla işlem yürütme kapsam birleştirerek gruplandırır. Doğası gereği, böyle bir işlem işlemlerinin belirli bir gruba ait tüm işlemlerin başarılı veya başarısız ortaklaşa emin olmalısınız.
İşlemler tarafından tanımlayıcı gruplandırılır `txn-id`.

İşlem etkileşim için istemci görevi gören bir `transaction controller` birlikte gruplandırılmalıdır işlemleri denetler. Service Bus hizmeti olarak görev yapan bir `transactional resource` ve iş tarafından istenen şekilde gerçekleştirir `transaction controller`.

İstemci ve hizmet üzerinden iletişim kurmak bir `control link` istemci tarafından oluşturulmuş. `declare` Ve `discharge` iletileri denetleyici tarafından ayırmak ve işlemleri sırasıyla tamamlamak için denetim bağlantı üzerinden gönderilen (bunlar işlem iş düzenleme temsil etmeyen). Gerçek Gönder/Al bu bağlantıyı yapılmaz. İstenen her işlem açıkça işlemdir istenen ile tanımlanan `txn-id` ve bu nedenle bağlantı herhangi bir bağlantıyı ortaya çıkabilir. Oluşturulduğu boşalmış olmayan işlemleri varken denetimi bağlantı kapattıysanız, ardından bu tür işlemler hemen geri alınır ve bunlar üzerinde daha fazla işlem iş gerçekleştirmeyi dener hatasına neden. Denetim bağlantı iletileri kapatılan öncesi olmaması gerekir.

Başlangıç ve bitiş işlemleri yapabilmek için kendi denetim bağlantı başlatmasını her bağlantısı vardır. Hizmet görür özel bir hedef tanımlayan bir `coordinator`. İstemci/denetleyicisi bu hedef denetim bağlantı kurar. Denetim bağlantı bir varlık sınır dışında başka bir deyişle, aynı denetim bağlantı başlatmak ve işlemleri için birden çok varlık Boşalma için kullanılabilir.

#### <a name="starting-a-transaction"></a>Bir işlem başlatılıyor

İşlem iş başlamak için. Denetleyici edinmelidir bir `txn-id` Düzenleyicisi'nden. Bunu göndererek yapar bir `declare` iletiyi yazın. Bildirim başarılı olursa, düzenleyici değerlendirme sonucu yanıt `declared` hangi taşıyan atanan `txn-id`.

| İstemci (denetleyicisi) | | Hizmet veri yolu (Düzenleyicisi) |
| --- | --- | --- |
| ekleme)<br/>adı = {bağlantı adı}<br/>... ,<br/>Rol =**gönderen**,<br/>Hedef =**Düzenleyicisi**<br/>) | ------> |  |
|  | <------ | ekleme)<br/>adı = {bağlantı adı}<br/>... ,<br/>target=Coordinator()<br/>) |
| Aktarım)<br/>teslim kimliği = 0,...)<br/>{AmqpValue (**Declare()**)}| ------> |  |
|  | <------ | Değerlendirme) <br/> İlk = 0, 0, son = <br/>Durum =**Declared**()<br/>**işlemlerinin kimliği**= {işlem kimliği}<br/>))|

#### <a name="discharging-a-transaction"></a>Bir işlem discharging

Denetleyici göndererek işlem iş ereceğini bir `discharge` Düzenleyici ileti. Denetleyici, yürütme veya geri alma için işlem iş ayarlayarak istediği olduğunu gösterir `fail` deşarj gövde bayrağı. Düzenleyici deşarj tamamlayamıyor ise, ileti bu sonucu taşıyan ile reddedilir `transaction-error`.

> Not: başarısız = true başvuran bir işlem ve başarısız geri almak için = false uygulanmak üzere başvuruyor.

| İstemci (denetleyicisi) | | Hizmet veri yolu (Düzenleyicisi) |
| --- | --- | --- |
| Aktarım)<br/>teslim kimliği = 0,...)<br/>{AmqpValue (Declare())}| ------> |  |
|  | <------ | Değerlendirme) <br/> İlk = 0, 0, son = <br/>Durum = bildirilen ()<br/>TXN kimliği = {işlem kimliği}<br/>))|
| | . . . <br/>İşlem çalışma<br/>diğer bağlantıları<br/> . . . |
| Aktarım)<br/>teslim kimliği = 57,...)<br/>{AmqpValue)<br/>**Boşalma (işlemlerinin kimliği = 0,<br/>başarısız = false)**)}| ------> |  |
| | <------ | Değerlendirme) <br/> ilk = 57, son 57, = <br/>Durum =**kabul()**)|

#### <a name="sending-a-message-in-a-transaction"></a>Bir işlem içinde ileti gönderme

Tüm işlem iş işlem teslim durumuyla yapılır `transactional-state` işlemlerinin kimliği taşır. İşlem-durumu iletileri gönderme söz konusu olduğunda iletinin Aktarım çerçevesi tarafından taşınır. 

| İstemci (denetleyicisi) | | Hizmet veri yolu (Düzenleyicisi) |
| --- | --- | --- |
| Aktarım)<br/>teslim kimliği = 0,...)<br/>{AmqpValue (Declare())}| ------> |  |
|  | <------ | Değerlendirme) <br/> İlk = 0, 0, son = <br/>Durum = bildirilen ()<br/>TXN kimliği = {işlem kimliği}<br/>))|
| Aktarım)<br/>tanıtıcı = 1,<br/>teslim kimliği = 1, <br/>**Durum =<br/>TransactionalState (<br/>txn kimliği = 0)**)<br/>{Yükü}| ------> |  |
| | <------ | Değerlendirme) <br/> ilk = 1, en son = 1, <br/>Durum =**TransactionalState (<br/>txn kimliği = 0,<br/>outcome=Accepted()**))|

#### <a name="disposing-a-message-in-a-transaction"></a>Bir işlemde bir ileti atma

İleti değerlendirme gibi işlemleri içeren `Complete`  /  `Abandon`  /  `DeadLetter`  /  `Defer`. Bir işlem içinde bu işlemleri gerçekleştirmek için geçirmek `transactional-state` değerlendirme ile.

| İstemci (denetleyicisi) | | Hizmet veri yolu (Düzenleyicisi) |
| --- | --- | --- |
| Aktarım)<br/>teslim kimliği = 0,...)<br/>{AmqpValue (Declare())}| ------> |  |
|  | <------ | Değerlendirme) <br/> İlk = 0, 0, son = <br/>Durum = bildirilen ()<br/>TXN kimliği = {işlem kimliği}<br/>))|
| | <------ |Aktarım)<br/>Tanıtıcı = 2,<br/>teslim Kimliği = 11, <br/>Durum = null)<br/>{Yükü}|  
| Değerlendirme) <br/> ilk = 11, son = 11, <br/>Durum =**TransactionalState (<br/>txn kimliği = 0,<br/>outcome=Accepted()**))| ------> |


## <a name="advanced-service-bus-capabilities"></a>Gelişmiş Service Bus özellikleri

Bu bölüm, Azure Service Bus'ın OASIS teknik komitesi AMQP için şu anda geliştirilmekte taslak uzantıları amqp, temel alan Gelişmiş özelliklerini kapsar. Service Bus bu Taslaklar en son sürümlerini uygular ve bu Taslaklar standart durumuna ulaşmasını olarak sunulan değişiklikler devralır.

> [!NOTE]
> Service Bus Mesajlaşma hizmeti Gelişmiş işlemler istek/yanıt modeli aracılığıyla desteklenir. Bu işlem ayrıntılarını makalesinde açıklanan [hizmet veri yolu AMQP 1.0: istek-yanıt tabanlı işlemleri](service-bus-amqp-request-response.md).
> 
> 

### <a name="amqp-management"></a>AMQP Yönetimi

AMQP yönetim bu makalede açıklanan taslak uzantıları ilk belirtimidir. Bu belirtimi bir AMQP yönetim etkileşimleri Mesajlaşma altyapısı ile izin AMQP protokolünü üzerinde katmanlanmış protokolleri kümesi tanımlar. Belirtimi genel işlemleri gibi tanımlayan *oluşturma*, *okuma*, *güncelleştirme*, ve *silme* bir ileti altyapısı ve sorgu işlemleri kümesi içindeki varlıklar yönetmek için.

Bu hareketleri Mesajlaşma altyapısı ile istemci arasında bir istek/yanıt etkileşimi gerektirir ve bu nedenle bu etkileşim düzeni AMQP üstünde modellemek nasıl belirtimi tanımlar: istemci Mesajlaşma altyapısı bağlandığında, bir oturum başlatır ve bağlantıları çifti oluşturur. Bir bağlantı üzerindeki istemci gönderen olarak davranır ve diğer bağlı alıcı, böylece bir çift yönlü kanalı olarak davranıp bağlantıları oluşturma gibi davranır.

| Mantıksal işlemi | İstemci | Service Bus |
| --- | --- | --- |
| İstek yanıt yolu oluşturun |--> () ekleme<br/>adı = {*bağlantı adı*},<br/>tanıtıcı = {*sayısal tanıtıcı*},<br/>Rol =**gönderen**,<br/>Kaynak =**null**,<br/>target = "myentity / $management"<br/>) |Eylem yok |
| İstek yanıt yolu oluşturun |Eylem yok |\<--ekleme ()<br/>adı = {*bağlantı adı*},<br/>tanıtıcı = {*sayısal tanıtıcı*},<br/>Rol =**alıcı**,<br/>Kaynak = null,<br/>target = "myentity"<br/>) |
| İstek yanıt yolu oluşturun |--> () ekleme<br/>adı = {*bağlantı adı*},<br/>tanıtıcı = {*sayısal tanıtıcı*},<br/>Rol =**alıcı**,<br/>Kaynak = "myentity / $management"<br/>target = "myclient$ kimliği"<br/>) | |
| İstek yanıt yolu oluşturun |Eylem yok |\<--ekleme ()<br/>adı = {*bağlantı adı*},<br/>tanıtıcı = {*sayısal tanıtıcı*},<br/>Rol =**gönderen**,<br/>Kaynak "myentity" =<br/>target = "myclient$ kimliği"<br/>) |

Bu bağlantılar çiftinin hazır olması, istek/yanıt uygulama basittir: bir varlık bu deseni anlar Mesajlaşma altyapısı içinde gönderilen bir iletinin isteğidir. Bu isteği-iletisinde, *Yanıtla* alanındaki *özellikleri* bölüm ayarlanmış *hedef* bağlantı yanıt teslim etmek için oturum tanımlayıcısı. İşleme varlık isteği işler ve ardından yanıt bağlantı üzerinden sunar, *hedef* tanımlayıcısıyla eşleşip belirtilen *Yanıtla* tanımlayıcısı.

Desen açıkça istemci kapsayıcı ve yanıt hedef istemci tarafından oluşturulan tanımlayıcısını tüm istemciler arasında ve güvenlik nedeniyle, tahmin etmek de zor benzersiz olmasını gerektirir.

Yönetim Protokolü'nün ve aynı desenini kullanan diğer tüm protokoller için kullanılan ileti alışverişlerinde uygulama düzeyinde gerçekleşir; Yeni AMQP protokol düzeyi hareketleri tanımlamayın. Böylece uygulamaları hemen, bu uzantıları ile uyumlu AMQP 1.0 yığınları yararlanabilir bilerek, olmasıdır.

Yönetim belirtim tarafından tanımlanan istek/yanıt düzeni talep tabanlı güvenlik özelliğinin ve neredeyse tüm aşağıdaki bölümlerde ele gelişmiş özellikler için temel ancak herhangi bir yönetim belirtimi çekirdek özelliklerini hizmet veri yolu şu anda uygulamaz.

### <a name="claims-based-authorization"></a>Talep tabanlı yetkilendirme

AMQP talep tabanlı yetkilendirme (CBS) belirtimi taslak yönetim belirtimi istek/yanıt desenine oluşturur ve Federasyon güvenlik belirteçleri ile AMQP kullanımı için genelleştirilmiş bir modeli açıklanmaktadır.

Girişte ele alınan AMQP varsayılan güvenlik modelinin SASL üzerinde temel alır ve AMQP bağlantı el sıkışması ile tümleşir. SASL kullanarak kendisi için bir dizi mekanizmaları tanımlanmışsa, resmi olarak SASL üzerinde leans herhangi bir iletişim kuralı yararlanabilir gelen genişletilebilir bir model sağlayan bir avantajı vardır. Bu mekanizmaları arasında kullanıcı adları ve parolalar, TLS düzeyi güvenlik için açık kimlik doğrulama/yetkilendirme olmaması ve çok çeşitli kimlik doğrulama ve/veya yetkilendirme kimlik bilgileri veya belirteçleri geçirme izni ek mekanizmaları ifade etmek için "anonim" bağlamak için "dış" transfer "DÜZ" içindir.

AMQP'ın SASL tümleştirme iki sakıncaları vardır:

* Tüm kimlik bilgilerini ve belirteçleri bağlantısı kapsamlıdır. Varlık başına temelinde farklı erişim denetimi sağlamak bir ileti altyapısı isteyebilirsiniz; Örneğin, bir kuyruk, ancak sıra B. göndermek bir belirteç taşıyıcı izin verme Kimlik doğrulama bağlamını bağlantısı bağlantılı, tek bir bağlantı ve henüz sırası A ve b sırası için farklı erişim belirteçleri kullanmak mümkün değildir
* Erişim belirteçleri genellikle yalnızca sınırlı bir süre için geçerlidir. Bu geçerlilik belirteçleri düzenli aralıklarla yeniden almak kullanıcının gerektiren ve kullanıcı erişim izinleri değiştirdiyseniz, yeni bir belirteç veren reddetmek için belirteç verenin fırsatı sunar. AMQP bağlantıları uzun süreler için son. SASL yalnızca ya da sahip belirtecin süresi dolduğunda mı, yoksa bir istemci ile sürekli iletişim vermeyle ilgili risk kabul etmesi gerekir istemci bağlantısını kesmek için ileti altyapısı anlamına bağlantı sırasında bir belirteç ayarlamak için bir fırsat modelidir kimin olan erişim haklarını geçici iptal.

Service Bus tarafından uygulanan AMQP CBS belirtimi zarif bir geçici çözüm hem de bu sorunları sağlar: bir istemci erişim belirteçleri her düğümle ilişkilendirilecek ve, ileti akışını kesintiye uğratmadan süresi dolmadan önce bu belirteçleri güncelleştirmeye izin verir.

CBS tanımlar adlı bir sanal yönetim düğümü *$cbs*, Mesajlaşma altyapısı tarafından sağlanacak. Yönetim düğümü Mesajlaşma altyapısı içindeki diğer düğümlere adına belirteçleri kabul eder.

Protokol hareketi yönetim belirtim tarafından tanımlanan istek/yanıt exchange ' dir. Anlamına gelir istemci bağlantılarıyla çifti oluşturur *$cbs* düğümü ve daha sonra bir istek giden bağlantıyı geçirir ve gelen bağlantıya yanıt için bekler.

İstek iletisi uygulama özellikleri şunlardır:

| Anahtar | İsteğe bağlı | Değer türü | Değer içeriği |
| --- | --- | --- | --- |
| işlemi |Hayır |string |**PUT-token** |
| type |Hayır |string |Put yöntemi Belirtecin türü. |
| ad |Hayır |string |"Belirteç uygulandığı hedef kitleyi". |
| süre sonu |Evet |timestamp |Belirteç süre sonu zamanı. |

*Adı* özelliği, belirteci olacaktır ilişkili varlık tanımlar. Hizmet veri yolundaki kuyruk veya konu başlığının/aboneliğinin yoludur. *Türü* özelliği belirteç türü tanımlar:

| Belirteç türü | Belirteç açıklaması | Gövde türü | Notlar |
| --- | --- | --- | --- |
| amqp:jwt |JSON Web Token (JWT) |AMQP değeri (dize) |Henüz kullanılamıyor. |
| amqp:swt |Basit Web Token (SWT) |AMQP değeri (dize) |Yalnızca desteklenen AAD/ACS tarafından yayınlanan SWT belirteçleri için |
| servicebus.Windows.NET:sastoken |Hizmet veri yolu SAS belirteci |AMQP değeri (dize) |- |

Belirteçleri hakları confer. Hizmet veri yolu bilir üç temel hakları hakkında: "Gönderme" etkinleştirir gönderme, "Dinleme" alma sağlar ve "Manage" bulmada varlıklar sağlar. AAD/ACS tarafından açıkça yayınlanan SWT belirteçleri talepler olarak bu hakları içerir. Hizmet veri yolu SAS belirteci ad alanı veya varlık yapılandırılan kuralları başvurun ve bu kuralları haklarıyla yapılandırılır. Bu kuralla ilişkili anahtarla belirteç imzalama böylece belirteci hızlı ilgili hakları yapar. Bir varlık kullanımıyla ilişkili belirteci *put belirteci* bağlı istemcinin belirteci hakları başına varlığı ile etkileşime izin verir. Burada istemci alır üzerinde bir bağlantı *gönderen* rolü gerektirir "Gönderme" sağ; almayı *alıcı* rolünün "Dinleme" hakkı olması gerekir.

Yanıt iletisi aşağıdaki sahip *uygulama özellikleri* değerleri

| Anahtar | İsteğe bağlı | Değer türü | Değer içeriği |
| --- | --- | --- | --- |
| Durum kodu |Hayır |Int |HTTP yanıt kodunu **[RFC2616]**. |
| Durum açıklaması |Evet |string |Durum açıklaması. |

İstemci çağırabilirsiniz *put belirteci* art arda ve mesajlaşma altyapısı herhangi bir varlık için. Belirteçler geçerli istemci için kapsamlı ve bağlantı düştüğünde sunucu tutulan herhangi bir belirtece bırakır anlamı geçerli bağlantıda bağlantılı.

Geçerli hizmet veri yolu uygulaması CBS yalnızca "Anonim" SASL yöntemiyle birlikte sağlar SSL/TLS bağlantısı her zaman SASL el sıkışma önce mevcut olması gerekir.

Anonim mekanizması, bu nedenle seçilen AMQP 1.0 istemcisi tarafından desteklenmelidir. İlk bağlantı el sıkışması ilk oturum oluşturma dahil olmak üzere hizmet veri yolu gerçekleşmesi anonim erişim kimin bağlantı oluşturma bilerek anlamına gelir.

Oturumu ve bağlantı kurulduğunda sonra bağlantılar ekleme *$cbs* düğümü ve gönderme *put belirteci* yalnızca verilen işlem isteği. Geçerli bir belirteci kullanarak başarıyla ayarlanmalıdır bir *put belirteci* istek bağlantı kurulduktan sonra bazı 20 saniye içinde varlık düğümü için Aksi halde bağlantı tek taraflı Service Bus tarafından düşer.

İstemci, daha sonra belirteç süre sonu izlemek için sorumludur. Bir belirtecinin süresi dolduğunda, Service Bus ilgili varlık bağlantısı üzerinde tüm bağlantılar derhal bırakır. Bunu önlemek için istemci belirtecin düğümü için yeni bir sanal aracılığıyla herhangi bir zamanda değiştirebilirsiniz *$cbs* aynı yönetim düğümle *put belirteci* hareket ve olmadan alma farklı bağlantıları akan yükü trafik in the way of.

### <a name="send-via-functionality"></a>Gönderme aracılığıyla işlevi

[Gönderme aracılığıyla / aktarım gönderen](service-bus-transactions.md#transfers-and-send-via) İleri belirli bir ileti başka bir varlık üzerinden bir hedef varlık için veri yolu hizmet sağlayan işlevdir. Bu özellik çoğunlukla tek bir işlemde varlıklar üzerinde işlemler gerçekleştirmek için kullanılır.

Bu işlevler sayesinde, bir gönderici oluşturun ve bağlantı kurmak `via-entity`. Bağlantı kurulurken ek bilgiler doğru hedef iletileri/aktarımlarının bu bağlantıyı kurmak için gönderilir. Ekleme Başarılı silindikten sonra bu bağlantıyı gönderilen tüm iletiler otomatik olarak iletilir *hedef varlık* aracılığıyla *varlık aracılığıyla*. 

> Not: Her ikisi için gerçekleştirilecek kimlik doğrulaması sahip *varlık aracılığıyla* ve *hedef varlık* Bu bağlantı kurmadan önce.

| İstemci | | Service Bus |
| --- | --- | --- |
| ekleme)<br/>adı = {bağlantı adı}<br/>Rol gönderenin =<br/>Kaynak = {istemci bağlantı kimliği}<br/>Hedef =**{varlık aracılığıyla}**,<br/>**Özellikler harita = [(<br/>com.microsoft:transfer hedef adres =<br/>{hedef varlık})]** ) | ------> | |
| | <------ | ekleme)<br/>adı = {bağlantı adı}<br/>Rol alıcı =<br/>Kaynak = {istemci bağlantı kimliği}<br/>Hedef {aracılığıyla varlık}, =<br/>Özellikler harita [() =<br/>com.Microsoft:transfer hedef adres =<br/>{Hedef varlık})] ) |

## <a name="next-steps"></a>Sonraki adımlar

AMQP hakkında daha fazla bilgi için aşağıdaki bağlantıları ziyaret edin:

* [Hizmet veri yolu AMQP genel bakış]
* [AMQP 1.0 desteği bölümlenmiş Service Bus kuyrukları ve konuları]
* [Windows Server için hizmet veri yolu AMQP]

[this video course]: https://www.youtube.com/playlist?list=PLmE4bZU0qx-wAP02i0I7PJWvDWoCytEjD
[1]: ./media/service-bus-amqp-protocol-guide/amqp1.png
[2]: ./media/service-bus-amqp-protocol-guide/amqp2.png
[3]: ./media/service-bus-amqp-protocol-guide/amqp3.png
[4]: ./media/service-bus-amqp-protocol-guide/amqp4.png

[Hizmet veri yolu AMQP genel bakış]: service-bus-amqp-overview.md
[AMQP 1.0 desteği bölümlenmiş Service Bus kuyrukları ve konuları]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[Windows Server için hizmet veri yolu AMQP]: https://msdn.microsoft.com/library/dn574799.aspx
