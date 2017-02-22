---
title: "Azure Event Hubs nedir ve neden kullanılır | Microsoft Docs"
description: "Azure Event Hubs’a genel bakış ve giriş - Web sitesi, uygulama ve cihazlardan bulut ölçekli telemetri alma"
services: event-hubs
documentationcenter: .net
author: banisadr
ms.assetid: 
ms.service: event-hubs
ms.topic: get-started-article
ms.date: 11/29/2016
ms.author: sethm; babanisa
translationtype: Human Translation
ms.sourcegitcommit: aa7244849f6286e8ef9f9785c133b4c326193c12
ms.openlocfilehash: 62eefb7a4591c712c5389d3ed7e5ff9675a80042


---
# <a name="what-is-azure-event-hubs"></a>Azure Event Hubs nedir?
Event Hubs saniyede milyonlarca olayı alabilen, yüksek oranda ölçeklenebilir bir veri akış platformudur. Bir Olay Hub’ına gönderilen veriler, herhangi bir gerçek zamanlı analiz sağlayıcısı ve toplu iş/depolama bağdaştırıcısı kullanılarak dönüştürülüp depolanabilir. Düşük gecikme ile yoğun ölçekte yayımlama-abonelik özellikleri sağlayabilen Event Hubs, Büyük Veriler için "kestirme yol" olarak görev yapar.

## <a name="why-use-event-hubs"></a>Event Hubs’ı neden kullanmalıyım?
Event Hubs olay ve telemetri işleme özellikler onu özellikle aşağıdaki durumlar için yararlı hale getirir:

* Uygulama izleme
* Kullanıcı deneyimi veya iş akışı işleme
* Nesnelerin İnterneti (IoT) senaryoları

Event Hubs ayrıca mobil uygulamalarda davranış işleme, web gruplarından trafik bilgileri, konsol oyunlarında oyun içi olay yakalama veya sanayi makinelerinden ya da bağlı taşıtlardan telemetri verileri toplamayı sağlar.

## <a name="azure-event-hubs-overview"></a>Azure Event Hubs’a genel bakış
Event Hubs’ın çözüm mimarilerinde oynadığı genel rol, bir olay ardışık düzeni için "ön kapı" olarak görev yapmalarıdır ve çoğunlukla *olay alıcı* olarak adlandırılırlar. Olay yutucu, bir olay akışının üretimini ilgili olayların kullanılmasına ayıran, olay yayımcıları ile olay tüketicileri arasında duran bir bileşen veya hizmettir.

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_full_pipeline.png)

Azure Event Hubs, düşük gecikme süresi ve yüksek güvenilirlik ile bulut ölçekli olay ve telemetri alımı sağlayan bir olay işleme hizmetidir. Event Hubs bir ileti akışı işleme yeteneği sağlar ve bir geleneksel kurumsal mesajlaşmadan çok farklı olan özelliklere sahiptir. Event Hubs özellikleri, yüksek işleme ve olay işleme senaryoları üzerine inşa edilmiştir. Bu nedenle Event Hubs, konu başlıkları gibi mesajlaşma varlıkları için sunulan bazı mesajlaşma özelliklerini uygulamaz.

Bir Olay Hub'ı, ad alanı düzeyinde oluşturulur ve birincil API arabirimi olarak AMQP ile HTTP kullanır.

## <a name="event-publishers"></a>Olay yayımcıları
Bir Olay Hub'ına veri gönderen herhangi bir varlık *olay yayımcısıdır*. Olay yayımcıları HTTPS veya AMQP 1.0 kullanarak olayları yayımlayabilir. Olay yayımcıları kendilerini bir Event Hub'ına tanıtmak için Paylaşılan Erişim İmzası (SAS) belirteci kullanır ve benzersiz bir kimliğe sahip olabilir ya da ortak bir SAS belirteci kullanabilir.

### <a name="publishing-an-event"></a>Olay yayımlama
Bir olayı AMQP 1.0 veya HTTPS üzerinden yayımlayabilirsiniz. Service Bus, .NET istemcilerinden bir Event Hub'ına olayları yayımlamak için [EventHubClient](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventhubclient) sınıfını sağlar. Diğer çalışma zamanları ve platformlar için [Apache Qpid](http://qpid.apache.org/) gibi herhangi bir AMQP 1.0 istemcisi kullanabilirsiniz. Olayları ayrı ayrı veya toplu olarak yayımlayabilirsiniz. Tek bir yayın (olay verileri örneği), tek bir olay ya da toplu işlem olmasına bakılmaksızın 256 KB sınırlamaya sahiptir. Bundan büyük olayların yayımlanması bir hatayla sonuçlanır. Yayımcıların Event Hub'ındaki bölümleri bilmemesi ve yalnızca bir *bölüm anahtarı* (sonraki bölümde açıklanmıştır) ya da kimliklerini SAS belirteci üzerinden belirtmeleri en iyi yöntemdir.

AMQP veya HTTPS kullanma seçimi kullanım senaryosuna bağlıdır. AMQP, taşıma düzeyi güvenliği (TLS) veya SSL/TLS’ye ek olarak kalıcı bir çift yönlü yuva oluşturulmasını gerektirir. Oturum başlatılırken AMQP’nin ağ maliyetleri daha yüksektir, ancak HTTPS her istek için ek SSL yükü gerektirir. Daha sık yayımcılar için AMQP daha yüksek performans sunar.

![Event Hubs](./media/event-hubs-what-is-event-hubs/partition_keys.png)

Event Hubs aynı bölüm anahtarı değerini paylaşan tüm olayların aynı sırayla ve aynı bölüme iletilmesini sağlar. Bölüm anahtarlarının yayımcı ilkeleriyle birlikte kullanılması durumunda yayımcı kimliğinin ve bölüm anahtarı değerinin eşleşmesi gerekir. Aksi takdirde bir hata oluşur.

### <a name="publisher-policy"></a>Yayımcı ilkesi
Event Hubs, *yayımcı ilkeleri* aracılığıyla olay yayımcıları üzerinde ayrıntılı denetim sağlar. Yayımcı ilkeleri çok sayıda bağımsız olay yayımcısını kolaylaştırmak için tasarlanmış çalışma zamanı özellikleridir. Yayımcı ilkeleriyle her yayımcı, olayları aşağıdaki mekanizmayı kullanarak bir Event Hub'ında yayımlarken kendi benzersiz tanımlayıcısını kullanır:

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Yayımcı adlarını önceden oluşturmanız gerekli değildir, ancak bunlar bağımsız yayımcı kimlikleri sağlamak amacıyla bir olayı yayımlarken kullanılan SAS belirteci ile eşleşmelidir. Yayımcı ilkelerini kullanırken **PartitionKey** değeri yayımcı adına ayarlanır. Bu hizmetin düzgün çalışması için bu değerlerin eşleşmesi gerekir.

## <a name="partitions"></a>Bölümler
Event Hubs her bir tüketicinin ileti akışında yalnızca belirli bir alt küme ya da bölümü okuduğu bölünmüş bir tüketici modeli aracılığıyla ileti akışı sağlar. Bu model, olay işleme için yatay ölçek sağlar ve kuyruklar ile konularda kullanılamayan diğer akış odaklı özellikleri sunar.

Bölüm bir Event Hub'ında tutulan olayların sıralı dizisidir. Yeni olaylar geldikçe dizinin sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

![Event Hubs](./media/event-hubs-what-is-event-hubs/partition.png)

Event Hubs, verileri Olay Hub’ındaki tüm bölümler için geçerli olan bir yapılandırılmış elde tutma süresi boyunca saklar. Olayların süresi saat bazında dolar; bunları açıkça silemezsiniz. Bölümler birbirinden bağımsız olup kendi veri dizisini içerdiğinden genellikle farklı hızlarda büyürler.

![Event Hubs](./media/event-hubs-what-is-event-hubs/multiple_partitions.png)

Bölüm sayısı, oluşturma sırasında belirtilir ve 2 ile 32 arasında olmalıdır. Bölüm sayısı değiştirilemez; bu nedenle, bölüm sayısını ayarlarken uzun vadeli ölçeği dikkate almanız gerekir. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir Olay Hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Event Hubs ekibine başvurarak bölüm sayısını 32’nin üzerine çıkarabilirsiniz.

Bölümler tanımlanabilir olup doğrudan gönderilebilse de bunun yapılması önerilmez. Bunun yerine, [Olay yayımcısı](#event-publishers) ve [Kapasite](#capacity) bölümlerinde sunulan daha yüksek düzeyli yapıları kullanabilirsiniz.

Bölümler olayın gövdesini, kullanıcı tarafından tanımlanan bir özellik paketini ve bölümdeki uzaklığı ile akış dizisindeki sayısı gibi olay verileri dizisiyle doldurulur.

### <a name="partition-key"></a>Bölüm anahtarı
Gelen olay verilerini veri düzenleme amacıyla belirli bölümlere eşlemek için bölüm anahtarı kullanabilirsiniz. Bölüm anahtarı, gönderen tarafından belirtilip bir Event Hub'ına geçirilen değerdir. Statik karma işlevi ile işlenir ve sonuçta bölüm ataması oluşturulur. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır.

Olay yayımcısı yalnızca bölüm anahtarını bilir, olayların yayımlandığı bölümü bilmez. Anahtar ile bölümün bu şekilde ayrılması göndereni aşağı akış işleme hakkında çok fazla bilgi sahibi olma gereksiniminden kurtarır. Cihaz veya kullanıcı başına benzersiz bir kimlik iyi bir bölüm anahtarı oluşturur, ancak ilgili olayları tek bir bölümde gruplandırmak için coğrafi bölge gibi diğer öznitelikler de kullanılabilir.

## <a name="sas-tokens"></a>SAS belirteçleri
Event Hubs, ad alanında ve Olay Hub’ı düzeyinde kullanılabilen *Paylaşılan Erişim İmzaları* kullanır. SAS belirteci bir SAS anahtarından oluşturulur ve belirli bir biçimde kodlanmış bir URL’nin SHA karmasıdır. Event Hubs anahtar (ilke) ve belirtecin adını kullanarak karmayı yeniden oluşturabilir ve böylece gönderenin kimliğini doğrular. Normalde, olay yayımcıları için SAS belirteci yalnızca belirli bir Event Hub'ı üzerindeki **gönder** ayrıcalıkları ile oluşturulur. Bu SAS belirteci URL mekanizması, yayımcı ilkesinde sunulan yayımcı kimliğinin temelini oluşturur. SAS ile çalışma hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus-messaging/service-bus-shared-access-signature-authentication.md).

## <a name="event-consumers"></a>Olay tüketicileri
Bir Olay Hub'ından olay verilerini okuyan herhangi bir varlık *olay tüketicisidir*. Tüm Event Hubs tüketicileri AMQP 1.0 oturumu üzerinden bağlanır ve olaylar, kullanılabilir olduğu anda oturum üzerinden iletilir. İstemcinin veri kullanılabilirliğini yoklaması gerekmez.

### <a name="consumer-groups"></a>Tüketici grupları
Event Hubs yayımlama/abonelik mekanizması *tüketici grupları* aracılığıyla etkinleştirilir. Tüketici grubu tüm Event Hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları birden çok tüketen uygulamayı her biri olay akışının ayrı bir görünümüne sahip olacak ve akışı kendi hızlarında ve kendi sapmalarıyla bağımsız bir şekilde okuyacak şekilde etkinleştirir.

Bir akış işleme mimarisinde her bir aşağı akış uygulaması bir tüketici grubuna karşılık gelir. Olay verilerini uzun süreli depolama alanına yazmak isterseniz bu depolama yazma uygulaması bir tüketici grubudur. Bundan sonra karmaşık olay işlemesi başka ve ayrı bir tüketici grubu tarafından gerçekleştirilebilir. Bölümlere yalnızca bir tüketici grubu üzerinden erişebilirsiniz. Her bölümde **belirli bir tüketici grubundan** aynı anda yalnızca bir etkin okuyucu olabilir. Bir Event Hub'ında her zaman varsayılan bir tüketici grubu vardır ve Standart katmanlı bir Event Hub'ı için en fazla 20 tüketici grubu oluşturabilirsiniz.

Tüketici grubu URI kuralının örnekleri aşağıda verilmiştir:

```
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

![Event Hubs](./media/event-hubs-what-is-event-hubs/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Akış uzaklıkları
*Uzaklık* bir olayın bölüm içindeki konumudur. Uzaklığı istemci tarafındaki bir imleç olarak düşünebilirsiniz. Uzaklık, olayın bayt cinsinden numaralandırılmasıdır. Bu değer, olay tüketicisinin (okuyucu) olay akışında olayları okumaya başlamak istediği bir noktayı belirtmesini sağlar. Uzaklığı bir zaman damgası veya bir uzaklık değeri olarak belirtebilirsiniz. Tüketiciler, kendi uzaklık değerlerini Event Hubs hizmetinin dışında saklamaktan sorumludur. Bir bölüm içinde her olay bir uzaklık içerir.

![Event Hubs](./media/event-hubs-what-is-event-hubs/partition_offset.png)

### <a name="checkpointing"></a>Denetim noktası oluşturma
*Denetim noktası oluşturma*, okuyucuların bir bölüm olay dizisindeki konumlarını işaretledikleri veya uyguladıkları bir işlemdir. Denetim noktası oluşturma, tüketicinin sorumluluğundadır ve bir tüketici grubunda bölüm başına temelinde gerçekleşir. Diğer bir deyişle, her bir tüketici grubu için her bölüm okuyucusu geçerli konumunu olay akışında izlemelidir ve veri akışının tamamlandığını düşündüğünde hizmeti bilgilendirebilir.

Bir okuyucunun bölüm bağlantısı kesilirse yeniden bağlandığında ilgili tüketici grubundaki o bölümün son okuyucusu tarafından daha önce gönderilen denetim noktasında okumaya başlar. Okuyucu bağlandığında okumaya başlayacağı konumu belirtmek üzere bu uzaklığı Event Hub'ına geçirir. Bu şekilde denetim noktası oluşturma özelliğini hem aşağı akış uygulamaları ile olayları "tamamlandı" olarak işaretlemek hem de farklı makinelerde çalışan okuyucular arasında bir yük devretme oluşması durumunda esneklik sağlamak amacıyla kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. Bu mekanizmayla denetim noktası oluşturma özelliği hem yük devretme esnekliği hem de olay akışı yeniden yürütmesi sağlar.

### <a name="common-consumer-tasks"></a>Ortak tüketici görevleri
Tüm Event Hubs tüketicileri bir AMQP 1.0 oturumu ile durum bilgisi olan iki yönlü iletişim kanalı üzerinden bağlanır. Her bölümde bölüme göre ayrılmış olayların taşınmasını kolaylaştıran bir AMQP 1.0 oturumu vardır.

#### <a name="connect-to-a-partition"></a>Bir bölüme bağlanma
Bölümlere doğrudan bağlanırken okuyucu bağlantılarının belirli bölümlerle koordine edilmesi için bir kiralama mekanizmasının kullanılması yaygın bir uygulamadır. Bu şekilde, bir tüketici grubundaki her bölümün yalnızca bir etkin okuyucuya sahip olması mümkündür. Denetim noktası oluşturma, kiralama ve okuyucuları yönetme işlemleri, .NET istemcileri için [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) sınıfı kullanılarak basitleştirilir. [EventProcessorHost](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) akıllı bir tüketici aracısıdır.

#### <a name="read-events"></a>Olayları okuma
Belirli bir bölüm için bir AMQP 1.0 oturumu ve bağlantı açıldıktan sonra olaylar Event Hubs hizmeti tarafından AMQP 1.0 istemcisine teslim edilir. Bu teslim mekanizması, HTTP GET gibi çekme tabanlı mekanizmalardan daha yüksek verimlilik ve daha düşük gecikme sağlar. Olaylar istemciye gönderildiğinde her bir olay verisi örneği, olay dizisinde denetim noktası oluşturmayı kolaylaştırmak için kullanılan uzaklık ve dizi numarası gibi önemli meta veriler içerir.

Olay verileri:
* Uzaklık
* Sıra numarası
* Gövde
* Kullanıcı özellikleri
* Sistem özellikleri

Uzaklığın yönetilmesi sizin sorumluluğunuzdadır.

## <a name="capacity"></a>Kapasite
Event Hubs yüksek oranda ölçeklenebilir bir mimaridir ve boyutlandırma ile ölçeklendirme sırasında göz önünde bulundurulması gereken birkaç temel faktör vardır.

### <a name="throughput-units"></a>İşleme birimleri
Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir işleme birimi aşağıdakileri içerir:

* Giriş: Saniyede 1 MB veya saniyede 1000 olaya kadar (hangisi önce gerçekleşirse)
* Çıkış: Saniyede 2 MB’ye kadar

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](https://docs.microsoft.com/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. İşleme birimlerini [Azure portal][Azure portal] ad alanlarının **Ölçek** dikey penceresinde yönetebilirsiniz. Bu işlem Azure API'leri kullanılarak programlı olarak da yapılabilir.

İşleme birimleri saat başına faturalandırılır ve önceden satın alınır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. Bir Event Hubs ad alanı için en fazla 20 işleme birimi satın alınabilir ve ad alanındaki tüm Olay Hub’larında paylaşılır.

Azure desteğine başvurularak 20’li bloklar halinde olacak şekilde 100’e kadar daha fazla işleme birimi satın alınabilir. Bundan sonraki miktarlar için 100 işleme biriminden oluşan bloklar satın alabilirsiniz.

En iyi ölçeği elde etmek için işleme birimleri ve bölümlerini dengelemeniz önerilir. Tek bir bölüm en fazla bir işleme biriminden oluşan ölçeğe sahiptir. İşleme birimlerinin sayısı bir Event Hub’ındaki bölüm sayısına eşit veya daha az olmalıdır.

Ayrıntılı fiyatlandırma bilgileri için bkz. [Event Hubs Fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Sonraki adımlar

* [Event Hubs öğreticisi][Event Hubs tutorial] ile çalışmaya başlama
* [Event Hubs kullanan bir örnek uygulamanın] tamamı.
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)

[Event Hubs tutorial]: event-hubs-csharp-ephcs-getstarted.md
[Event Hubs kullanan bir örnek uygulamanın]: https://code.msdn.microsoft.com/Service-Bus-Event-Hub-286fd097
[Azure portal]: https://portal.azure.com



<!--HONumber=Feb17_HO1-->


