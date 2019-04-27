---
title: Özellikler - Azure Event Hubs'a genel bakış | Microsoft Docs
description: Bu makalede, özellikleri ve Azure Event Hubs'ın terminolojisi hakkında ayrıntılar sağlar.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.custom: seodec18
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: e7f292db06d4da9206aabd14a68e6acde867f92d
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2019
ms.locfileid: "60822000"
---
# <a name="features-and-terminology-in-azure-event-hubs"></a>Özellikler ve Azure Event Hubs terminolojisinde

Azure Event Hubs, ölçeklenebilir bir olay işleme alır ve büyük hacimli olayları ve verileri, düşük gecikme süresi ve yüksek güvenilirlikle işler hizmetidir. Bkz: [Event Hubs nedir?](event-hubs-what-is-event-hubs.md) üst düzey bir genel bakış.

Bu makalede yer alan bilgiler geliştirir [genel bakış makalesi](event-hubs-what-is-event-hubs.md)ve Event Hubs bileşenler ve özellikler hakkında teknik ve uygulama ayrıntılarını sağlar.

## <a name="namespace"></a>Ad Alanı
Bir Event Hubs ad alanı tarafından başvurulan bir benzersiz bir kapsam kapsayıcı sağlar. kendi [tam etki alanı adı](https://en.wikipedia.org/wiki/Fully_qualified_domain_name), içinde bir veya daha fazla event hubs'ı veya Kafka konularını oluşturma. 

## <a name="event-hubs-for-apache-kafka"></a>Apache Kafka için Event Hubs

[Bu özellik](event-hubs-for-kafka-ecosystem-overview.md) müşterilerin Event Hubs'a Kafka protokolünü kullanarak iletişim kurmasına olanak tanır bir uç nokta sağlar. Bu tümleştirme, müşterilere bir Kafka uç noktası sağlar. Bu, müşterilerin kendi Kafka kümelerini çalıştırmak için bir alternatif sunar, Event Hubs konuşabilir mevcut Kafka uygulamalarını yapılandırmak sağlar. Event Hubs için Apache Kafka, Kafka protokolü 1.0 ve üzeri destekler. 

Bu tümleştirme sayesinde Kafka kümelerini çalıştırmak veya bunları içeren Zookeeper yönetmeniz gerekmez. Bu ayrıca, En zorlu olay hub'ları gibi yakalama, otomatik şişme ve coğrafi olağanüstü durum kurtarma özelliklerinin bazılarını ile çalışmanıza olanak sağlar.

Bu tümleştirme de yansıtma Oluşturucu gibi uygulamalar veya framework Kafka bağlanma gibi çalışması yalnızca yapılandırma değişikliğiyle clusterless sağlar. 

## <a name="event-publishers"></a>Olay yayımcıları

Olay hub'ına veri gönderen herhangi bir olay üretici varlıktır veya *olay yayımcısı*. Olay yayımcıları HTTPS veya AMQP 1.0 veya Kafka 1.0 ve üzeri kullanarak olayları yayımlayabilir. Olay yayımcıları kendilerini bir olay hub'ına tanıtmak için Paylaşılan Erişim İmzası (SAS) belirteci kullanır ve benzersiz bir kimliğe sahip olabilir ya da ortak bir SAS belirteci kullanabilir.

### <a name="publishing-an-event"></a>Olay yayımlama

Bir olayı AMQP 1.0, 1.0 (ve üzeri) Kafka veya HTTPS üzerinden yayımlayabilirsiniz. Event Hubs sağlar [istemci kitaplıkları ve sınıfları](event-hubs-dotnet-framework-api-overview.md) olayları, .NET istemcilerinden bir olay hub'ına yayımlama. Diğer çalışma zamanları ve platformlar için [Apache Qpid](https://qpid.apache.org/) gibi herhangi bir AMQP 1.0 istemcisi kullanabilirsiniz. Olayları ayrı ayrı veya toplu olarak yayımlayabilirsiniz. Tek bir yayın (Olay verileri örneği), tek bir olay ya da toplu işlem olmasına bakılmaksızın 1 MB sınırı vardır. Bu hata eşiği sonuçlarında daha büyük olaylar yayımlama. Yayımcıların olay hub'ındaki bölümleri bilmemesi ve yalnızca bir *bölüm anahtarı* (sonraki bölümde açıklanmıştır) ya da kimliklerini SAS belirteci üzerinden belirtmeleri en iyi yöntemdir.

AMQP veya HTTPS kullanma seçimi kullanım senaryosuna bağlıdır. AMQP, taşıma düzeyi güvenliği (TLS) veya SSL/TLS’ye ek olarak kalıcı bir çift yönlü yuva oluşturulmasını gerektirir. Oturum başlatılırken AMQP’nin ağ maliyetleri daha yüksektir, ancak HTTPS her istek için ek SSL yükü gerektirir. Daha sık yayımcılar için AMQP daha yüksek performans sunar.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Event Hubs aynı bölüm anahtarı değerini paylaşan tüm olayların sırayla ve aynı bölüme iletilmesini sağlar. Bölüm anahtarlarının yayımcı ilkeleriyle birlikte kullanılması durumunda yayımcı kimliğinin ve bölüm anahtarı değerinin eşleşmesi gerekir. Aksi takdirde bir hata oluşur.

### <a name="publisher-policy"></a>Yayımcı ilkesi

Event Hubs, *yayımcı ilkeleri* aracılığıyla olay yayımcıları üzerinde ayrıntılı denetim sağlar. Yayımcı ilkeleri çok sayıda bağımsız olay yayımcısını kolaylaştırmak için tasarlanmış çalışma zamanı özellikleridir. Yayımcı ilkeleriyle her yayımcı, olayları aşağıdaki mekanizmayı kullanarak bir olay hub'ında yayımlarken kendi benzersiz tanımlayıcısını kullanır:

```http
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Yayımcı adlarını önceden oluşturmanız gerekli değildir, ancak bunlar bağımsız yayımcı kimlikleri sağlamak amacıyla bir olayı yayımlarken kullanılan SAS belirteci ile eşleşmelidir. Yayımcı ilkelerini kullanırken **PartitionKey** değeri yayımcı adına ayarlanır. Bu hizmetin düzgün çalışması için bu değerlerin eşleşmesi gerekir.

## <a name="capture"></a>Capture

[Event Hubs yakalama](event-hubs-capture-overview.md) otomatik olarak Event Hubs, akış verilerini yakalamanıza ve seçtiğiniz bir Blob Depolama hesabı veya bir Azure veri Gölü hizmeti hesabı için kaydetmeden olanak tanır. Azure portalından yakalamayı etkinleştirme ve en küçük boyut ve yakalama gerçekleştirmek için zaman penceresi belirtin. Event Hubs yakalama özelliğini kullanarak, kendi Azure Blob Depolama hesabı ve kapsayıcı ya da bunlardan biri yakalanan verileri depolamak için kullanılan Azure veri Gölü hizmeti hesabı belirtin. Yakalanan veriler Apache Avro biçiminde yazılır.

## <a name="partitions"></a>Bölümler

Event Hubs her bir tüketicinin ileti akışında yalnızca belirli bir alt küme ya da bölümü okuduğu bölünmüş bir tüketici modeli aracılığıyla ileti akışı sağlar. Bu model, olay işleme için yatay ölçek sağlar ve kuyruklar ile konularda kullanılamayan diğer akış odaklı özellikleri sunar.

Bölüm bir olay hub'ında tutulan olayların sıralı dizisidir. Yeni olaylar geldikçe dizinin sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

![Event Hubs](./media/event-hubs-features/partition.png)

Olay hub'ları, tüm bölümler, olay hub'ı uygulayan bir yapılandırılmış elde tutma süresi verilerini korur. Olayların süresi saat bazında dolar; bunları açıkça silemezsiniz. Bölümler birbirinden bağımsız olup kendi veri dizisini içerdiğinden genellikle farklı hızlarda büyürler.

![Event Hubs](./media/event-hubs-features/multiple_partitions.png)

Bölüm sayısı, oluşturma sırasında belirtilir ve 2 ile 32 arasında olmalıdır. Bölüm sayısı değiştirilemez; bu nedenle, bölüm sayısını ayarlarken uzun vadeli ölçeği dikkate almanız gerekir. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir olay hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Event Hubs ekibine başvurarak bölüm sayısını 32’nin üzerine çıkarabilirsiniz.

Bölümler tanımlanabilir ve doğrudan gönderilebilir olsa da doğrudan bir bölüme göndermek önerilmez. Bunun yerine, sunulan daha yüksek düzeyli yapıları kullanabilirsiniz [olay yayımcısı](#event-publishers) ve kapasite bölümler. 

Bölümler, olayı, kullanıcı tanımlı bir özellik paketini ve bölümdeki uzaklığı ve akış dizisindeki sayısı gibi meta verileri gövdesi içerir olay verileri dizisi ile doldurulur.

Bölümleri ve kullanılabilirlikleri ile güvenilirlikleri arasındaki dengeleme hakkında daha fazla bilgi için, bkz: [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md#partition-key) ve [Event Hubs’ta kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md) makalesi.

### <a name="partition-key"></a>Bölüm anahtarı

Gelen olay verilerini veri düzenleme amacıyla belirli bölümlere eşlemek için [bölüm anahtarı](event-hubs-programming-guide.md#partition-key) kullanabilirsiniz. Bölüm anahtarı, gönderen tarafından belirtilip bir olay hub'ına geçirilen değerdir. Statik karma işlevi ile işlenir ve sonuçta bölüm ataması oluşturulur. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır.

Olay yayımcısı yalnızca bölüm anahtarını bilir, olayların yayımlandığı bölümü bilmez. Anahtar ile bölümün bu şekilde ayrılması göndereni aşağı akış işleme hakkında çok fazla bilgi sahibi olma gereksiniminden kurtarır. Cihaz veya kullanıcı başına benzersiz bir kimlik iyi bir bölüm anahtarı oluşturur, ancak ilgili olayları tek bir bölümde gruplandırmak için coğrafi bölge gibi diğer öznitelikler de kullanılabilir.

## <a name="sas-tokens"></a>SAS belirteçleri

Event Hubs, ad alanında ve olay hub’ı düzeyinde kullanılabilen *Paylaşılan Erişim İmzaları* kullanır. SAS belirteci bir SAS anahtarından oluşturulur ve belirli bir biçimde kodlanmış bir URL’nin SHA karmasıdır. Event Hubs anahtar (ilke) ve belirtecin adını kullanarak karmayı yeniden oluşturabilir ve böylece gönderenin kimliğini doğrular. Normalde, olay yayımcıları için SAS belirteci yalnızca belirli bir olay hub'ı üzerindeki **gönder** ayrıcalıkları ile oluşturulur. Bu SAS belirteci URL mekanizması, yayımcı ilkesinde sunulan yayımcı kimliğinin temelini oluşturur. SAS ile çalışma hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus-messaging/service-bus-sas.md).

## <a name="event-consumers"></a>Olay tüketicileri

Bir olay hub'ından olay verilerini okuyan herhangi bir varlık *olay tüketicisidir*. Tüm Event Hubs tüketicileri AMQP 1.0 oturumu üzerinden bağlanır ve olaylar, kullanılabilir olduğu anda oturum üzerinden iletilir. İstemcinin veri kullanılabilirliğini yoklaması gerekmez.

### <a name="consumer-groups"></a>Tüketici grupları

Event Hubs yayımlama/abonelik mekanizması *tüketici grupları* aracılığıyla etkinleştirilir. Tüketici grubu tüm olay hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları birden çok tüketen uygulamayı her biri olay akışının ayrı bir görünümüne sahip olacak ve akışı kendi hızlarında ve kendi sapmalarıyla bağımsız bir şekilde okuyacak şekilde etkinleştirir.

Bir akış işleme mimarisinde her bir aşağı akış uygulaması bir tüketici grubuna karşılık gelir. Olay verilerini uzun süreli depolama alanına yazmak isterseniz bu depolama yazma uygulaması bir tüketici grubudur. Bundan sonra karmaşık olay işlemesi başka ve ayrı bir tüketici grubu tarafından gerçekleştirilebilir. Bölümlere yalnızca bir tüketici grubu üzerinden erişebilirsiniz. Bir olay hub'ında her zaman varsayılan bir tüketici grubu vardır ve Standart katmanlı bir olay hub'ı için en fazla 20 tüketici grubu oluşturabilirsiniz.

Olabilir en fazla 5 eşzamanlı okuyucu tüketici grubu başına bir bölüme; ancak **olduğunu yalnızca bir etkin alıcı tüketici grubu başına bir bölüme önerilir**. Tek bir bölüm içinde her Okuyucu tüm iletileri alır. Ardından aynı bölüme birden fazla okuyucuyu kapsayacak varsa, yinelenen iletileri işler. Bu Önemsiz olmayabilir, kodunuzda ele almanız gerekir. Ancak, bazı senaryolarda geçerli bir yaklaşım değildir.


Tüketici grubu URI kuralının örnekleri aşağıda verilmiştir:

```http
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #1]
//[my namespace].servicebus.windows.net/[event hub name]/[Consumer Group #2]
```

Aşağıdaki şekilde Event Hubs akış işleme mimarisi gösterilmektedir:

![Event Hubs](./media/event-hubs-features/event_hubs_architecture.png)

### <a name="stream-offsets"></a>Akış uzaklıkları

*Uzaklık* bir olayın bölüm içindeki konumudur. Uzaklığı istemci tarafındaki bir imleç olarak düşünebilirsiniz. Uzaklık, olayın bayt cinsinden numaralandırılmasıdır. Bu uzaklık, olay tüketicisinin (okuyucu) olay akışında olayları okumaya başlamak istediği bir noktayı belirtmesini sağlar. Uzaklığı bir zaman damgası veya bir uzaklık değeri olarak belirtebilirsiniz. Tüketiciler, kendi uzaklık değerlerini Event Hubs hizmetinin dışında saklamaktan sorumludur. Bir bölüm içinde her olay bir uzaklık içerir.

![Event Hubs](./media/event-hubs-features/partition_offset.png)

### <a name="checkpointing"></a>Denetim noktası oluşturma

*Denetim noktası oluşturma*, okuyucuların bir bölüm olay dizisindeki konumlarını işaretledikleri veya uyguladıkları bir işlemdir. Denetim noktası oluşturma, tüketicinin sorumluluğundadır ve bir tüketici grubunda bölüm başına temelinde gerçekleşir. Bu sorumluluk, her bir tüketici grubu için her bölüm okuyucusunun geçerli konumunu olay akışında izlemesi gerektiği ve veri akışının tamamlandığını düşündüğünde hizmeti bilgilendirebileceği anlamına gelir.

Bir okuyucunun bölüm bağlantısı kesilirse yeniden bağlandığında ilgili tüketici grubundaki o bölümün son okuyucusu tarafından daha önce gönderilen denetim noktasında okumaya başlar. Okuyucu bağlandığında okumaya başlayacağı konumu belirtmek için olay hub'ına uzaklığı geçirir. Bu şekilde, denetim noktası oluşturma özelliğini hem aşağı akış uygulamaları ile olayları "tamamlandı" olarak işaretlemek hem de farklı makinelerde çalışan okuyucular arasında bir yük devretme oluşması durumunda esneklik sağlamak amacıyla kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. Bu mekanizmayla denetim noktası oluşturma özelliği hem yük devretme esnekliği hem de olay akışı yeniden yürütmesi sağlar.

### <a name="common-consumer-tasks"></a>Ortak tüketici görevleri

Tüm Event Hubs tüketicileri bir durumu algılayan çift yönlü iletişim kanalı bir AMQP 1.0 oturumu üzerinden bağlanır. Her bölümde bölüme göre ayrılmış olayların taşınmasını kolaylaştıran bir AMQP 1.0 oturumu vardır.

#### <a name="connect-to-a-partition"></a>Bir bölüme bağlanma

Bölümlere doğrudan bağlanırken okuyucu bağlantılarının belirli bölümlerle koordine edilmesi için bir kiralama mekanizmasının kullanılması yaygın bir uygulamadır. Bu şekilde, bir tüketici grubundaki her bölümün yalnızca bir etkin okuyucuya sahip olması mümkündür. Denetim noktası oluşturma, kiralama ve okuyucuları yönetme işlemleri, .NET istemcileri için [EventProcessorHost](/dotnet/api/microsoft.azure.eventhubs.processor.eventprocessorhost) sınıfı kullanılarak basitleştirilir. Event Processor Host, akıllı bir tüketici aracısıdır.

#### <a name="read-events"></a>Olayları okuma

Belirli bir bölüm için bir AMQP 1.0 oturumu ve bağlantı açıldıktan sonra olaylar Event Hubs hizmeti tarafından AMQP 1.0 istemcisine teslim edilir. Bu teslim mekanizması, HTTP GET gibi çekme tabanlı mekanizmalardan daha yüksek verimlilik ve daha düşük gecikme sağlar. Olaylar istemciye gönderildiğinde her bir olay verisi örneği, olay dizisinde denetim noktası oluşturmayı kolaylaştırmak için kullanılan uzaklık ve dizi numarası gibi önemli meta veriler içerir.

Olay verileri:
* Uzaklık
* Sıra numarası
* Gövde
* Kullanıcı özellikleri
* Sistem özellikleri

Uzaklığın yönetilmesi sizin sorumluluğunuzdadır.

## <a name="scaling-with-event-hubs"></a>Event Hubs ile ölçeklendirme

Event Hubs ile ölçeklendirme etkileyen iki faktörleri vardır.
*   İşleme birimleri
*   Bölümler

### <a name="throughput-units"></a>İşleme birimleri

Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir aktarım hızı sağlar:

* Giriş: İkinci veya 1000 olaya (hangisi önce gerçekleşirse) saniye başına başına 1 MB'a kadar.
* Çıkış: İkinci veya 4096 olay / saniye başına 2 MB'a kadar.

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](/dotnet/api/microsoft.azure.eventhubs.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. Üretilen iş birimleri yönetebileceğiniz **ölçek** alanlarının dikey [Azure portalında](https://portal.azure.com). Üretilen iş birimleri program aracılığıyla kullanarak da yönetebilirsiniz [olay hub'ları API](event-hubs-api-overview.md).

İşleme birimleri önceden satın alınır ve saat başına faturalandırılır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. En fazla 20 işleme birimi bir Event Hubs ad alanı için satın alınabilir ve bu ad alanındaki tüm event hubs arasında paylaşılır.

### <a name="partitions"></a>Bölümler

Bölümler izin verin, Ölçek, aşağı akış işleme için. Event Hubs bölümlerle sunan bölümlenmiş tüketici modelinin nedeniyle, olaylarınızı aynı anda işlenirken ölçeklendirme. Bir olay hub'ı 32 adede kadar bölümlere sahip olabilir.

En iyi ölçeği elde etmek için 1:1 işleme birimleri ve bölümlerini dengelemeniz önerilir. Garantili bir giriş ve çıkış en fazla bir işleme biriminden oluşan tek bir bölüm vardır. Bir bölüme daha yüksek performans sağlamak olabilir, ancak performans garanti edilmez. Bir olay hub'ındaki bölüm sayısı en az üretilen iş birimlerinin sayısı için önerilir nedeni budur.

Toplam aktarım hızı gerektiren üzerinde planlama göz önünde bulundurulduğunda, ihtiyaç duyduğunuz üretilen iş birimlerinin sayısı ve en düşük bölüm sayısı, ancak kaç bölümler gerekir biliyor musunuz? Gelecekteki bir üretilen iş hacmi gereksinimlerinizi yanı sıra ulaşmak istediğiniz aşağı akış paralelliğiyle üzerinde göre bölüm seçin. Sahip olduğunuz bir olay hub'ı bölüm sayısı için ücret alınmaz.

Event Hubs ayrıntılı fiyatlandırma bilgileri için bkz. [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi][Event Hubs tutorial] ile çalışmaya başlama
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Event Hubs örnekleri][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Event Hubs örnekleri]: https://github.com/Azure/azure-event-hubs/tree/master/samples
