---
title: "Azure Event Hubs özelliklere genel bakış | Microsoft Docs"
description: "Genel bakış ve Azure Event Hubs özellikleri hakkında ayrıntılı bilgileri"
services: event-hubs
documentationcenter: .net
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2017
ms.author: sethm
ms.openlocfilehash: a74d767d57eb5ce2b3a716f9ba908a451f25f538
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
---
# <a name="event-hubs-features-overview"></a>Olay hub'ları özelliklere genel bakış

Azure Event Hubs işleme alır ve düşük gecikme süreli ve yüksek güvenilirlikle olayları ve verileri, büyük miktarlarda işleyen hizmeti ölçeklenebilir bir olaydır. Bkz: [Event Hubs nedir?](event-hubs-what-is-event-hubs.md) hizmet üst düzey bir genel bakış için.

Bu makalede bilgileri derlemeler [genel bakış](event-hubs-what-is-event-hubs.md)ve Event Hubs bileşenleri ve özellikleri hakkında teknik ve uygulama ayrıntılar sağlar.

## <a name="event-publishers"></a>Olay yayımcıları

Olay hub'ına veri gönderen herhangi bir olay üretici varlıktır veya *olay yayımcısı*. Olay yayımcıları HTTPS veya AMQP 1.0 kullanarak olayları yayımlayabilir. Olay yayımcıları kendilerini bir olay hub'ına tanıtmak için Paylaşılan Erişim İmzası (SAS) belirteci kullanır ve benzersiz bir kimliğe sahip olabilir ya da ortak bir SAS belirteci kullanabilir.

### <a name="publishing-an-event"></a>Olay yayımlama

Bir olayı AMQP 1.0 veya HTTPS üzerinden yayımlayabilirsiniz. Event Hubs sağlar [istemci kitaplıkları ve sınıfları](event-hubs-dotnet-framework-api-overview.md) .NET istemcilerinden bir event hub'ına olayları yayımlamak için. Diğer çalışma zamanları ve platformlar için [Apache Qpid](http://qpid.apache.org/) gibi herhangi bir AMQP 1.0 istemcisi kullanabilirsiniz. Olayları ayrı ayrı veya toplu olarak yayımlayabilirsiniz. Tek bir yayın (olay verileri örneği), tek bir olay ya da toplu işlem olmasına bakılmaksızın 256 KB sınırlamaya sahiptir. Bu bir hata eşiği sonuçlarında büyük yayımlama olaylar. Yayımcıların olay hub'ındaki bölümleri bilmemesi ve yalnızca bir *bölüm anahtarı* (sonraki bölümde açıklanmıştır) ya da kimliklerini SAS belirteci üzerinden belirtmeleri en iyi yöntemdir.

AMQP veya HTTPS kullanma seçimi kullanım senaryosuna bağlıdır. AMQP, taşıma düzeyi güvenliği (TLS) veya SSL/TLS’ye ek olarak kalıcı bir çift yönlü yuva oluşturulmasını gerektirir. Oturum başlatılırken AMQP’nin ağ maliyetleri daha yüksektir, ancak HTTPS her istek için ek SSL yükü gerektirir. Daha sık yayımcılar için AMQP daha yüksek performans sunar.

![Event Hubs](./media/event-hubs-features/partition_keys.png)

Event Hubs aynı bölüm anahtarı değerini paylaşan tüm olayların sırayla ve aynı bölüme iletilmesini sağlar. Bölüm anahtarlarının yayımcı ilkeleriyle birlikte kullanılması durumunda yayımcı kimliğinin ve bölüm anahtarı değerinin eşleşmesi gerekir. Aksi takdirde bir hata oluşur.

### <a name="publisher-policy"></a>Yayımcı ilkesi

Event Hubs, *yayımcı ilkeleri* aracılığıyla olay yayımcıları üzerinde ayrıntılı denetim sağlar. Yayımcı ilkeleri çok sayıda bağımsız olay yayımcısını kolaylaştırmak için tasarlanmış çalışma zamanı özellikleridir. Yayımcı ilkeleriyle her yayımcı, olayları aşağıdaki mekanizmayı kullanarak bir olay hub'ında yayımlarken kendi benzersiz tanımlayıcısını kullanır:

```
//[my namespace].servicebus.windows.net/[event hub name]/publishers/[my publisher name]
```

Yayımcı adlarını önceden oluşturmanız gerekli değildir, ancak bunlar bağımsız yayımcı kimlikleri sağlamak amacıyla bir olayı yayımlarken kullanılan SAS belirteci ile eşleşmelidir. Yayımcı ilkelerini kullanırken **PartitionKey** değeri yayımcı adına ayarlanır. Bu hizmetin düzgün çalışması için bu değerlerin eşleşmesi gerekir.

## <a name="capture"></a>Capture

[Olay hub'ları yakalama](event-hubs-capture-overview.md) otomatik olarak olay hub'ları akış verilerini yakalamak ve için tercih ettiğiniz bir Blob storage hesabı veya bir Azure Data Lake hizmeti hesabının kaydetmenize olanak sağlar. Azure portalından yakalamayı etkinleştirme ve en küçük boyut ve yakalama gerçekleştirmek için zaman penceresini belirtin. Olay hub'ları yakalama kullanarak kendi Azure Blob Storage hesabı ve kapsayıcı ya da yakalanan verileri depolamak için kullanılan Azure Data Lake Service hesabı belirtin. Yakalanan veriler Apache Avro biçiminde yazılır.

## <a name="partitions"></a>Bölümler

Event Hubs her bir tüketicinin ileti akışında yalnızca belirli bir alt küme ya da bölümü okuduğu bölünmüş bir tüketici modeli aracılığıyla ileti akışı sağlar. Bu model, olay işleme için yatay ölçek sağlar ve kuyruklar ile konularda kullanılamayan diğer akış odaklı özellikleri sunar.

Bölüm bir olay hub'ında tutulan olayların sıralı dizisidir. Yeni olaylar geldikçe dizinin sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

![Event Hubs](./media/event-hubs-features/partition.png)

Olay hub'ları tüm bölümler olay hub'ı uygular yapılandırılan saklama süresi için verileri saklar. Olayların süresi saat bazında dolar; bunları açıkça silemezsiniz. Bölümler birbirinden bağımsız olup kendi veri dizisini içerdiğinden genellikle farklı hızlarda büyürler.

![Event Hubs](./media/event-hubs-features/multiple_partitions.png)

Bölüm sayısı, oluşturma sırasında belirtilir ve 2 ile 32 arasında olmalıdır. Bölüm sayısı değiştirilemez; bu nedenle, bölüm sayısını ayarlarken uzun vadeli ölçeği dikkate almanız gerekir. Bölümler, tüketen uygulamalarda gerekli aşağı akış paralelliğiyle ilişkili bir veri düzenleme mekanizmasıdır. Bir olay hub'ındaki bölüm sayısı, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgilidir. Event Hubs ekibine başvurarak bölüm sayısını 32’nin üzerine çıkarabilirsiniz.

Bölümler tanımlanabilir ve doğrudan gönderilebilir olsa da, doğrudan bir bölüm gönderme önerilmez. Bunun yerine, [Olay yayımcısı](#event-publishers) ve [Kapasite](#capacity) bölümlerinde sunulan daha yüksek düzeyli yapıları kullanabilirsiniz. 

Bölümler olayın gövdesini, kullanıcı tarafından tanımlanan bir özellik paketini ve bölümdeki uzaklığı ile akış dizisindeki sayısı gibi meta veriler dizisiyle doldurulur.

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

Bir akış işleme mimarisinde her bir aşağı akış uygulaması bir tüketici grubuna karşılık gelir. Olay verilerini uzun süreli depolama alanına yazmak isterseniz bu depolama yazma uygulaması bir tüketici grubudur. Bundan sonra karmaşık olay işlemesi başka ve ayrı bir tüketici grubu tarafından gerçekleştirilebilir. Bölümlere yalnızca bir tüketici grubu üzerinden erişebilirsiniz. Olabilir en fazla 5 eşzamanlı okuyucu tüketici grubu başına bir bölüme; ancak **olduğundan yalnızca bir etkin alıcı tüketici grubu başına bir bölüme önerilir**. Bir olay hub'ında her zaman varsayılan bir tüketici grubu vardır ve Standart katmanlı bir olay hub'ı için en fazla 20 tüketici grubu oluşturabilirsiniz.

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

Bir okuyucunun bölüm bağlantısı kesilirse yeniden bağlandığında ilgili tüketici grubundaki o bölümün son okuyucusu tarafından daha önce gönderilen denetim noktasında okumaya başlar. Okuyucu bağlandığında okumaya başlayacağı konumu belirtmek üzere bu uzaklığı olay hub'ına geçirir. Bu şekilde, denetim noktası oluşturma özelliğini hem aşağı akış uygulamaları ile olayları "tamamlandı" olarak işaretlemek hem de farklı makinelerde çalışan okuyucular arasında bir yük devretme oluşması durumunda esneklik sağlamak amacıyla kullanabilirsiniz. Bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. Bu mekanizmayla denetim noktası oluşturma özelliği hem yük devretme esnekliği hem de olay akışı yeniden yürütmesi sağlar.

### <a name="common-consumer-tasks"></a>Ortak tüketici görevleri

Tüm Event Hubs tüketicileri durumu algılayan çift yönlü iletişim kanalını bir AMQP 1.0 oturumu üzerinden bağlanır. Her bölümde bölüme göre ayrılmış olayların taşınmasını kolaylaştıran bir AMQP 1.0 oturumu vardır.

#### <a name="connect-to-a-partition"></a>Bir bölüme bağlanma

Bölümlere doğrudan bağlanırken okuyucu bağlantılarının belirli bölümlerle koordine edilmesi için bir kiralama mekanizmasının kullanılması yaygın bir uygulamadır. Bu şekilde, bir tüketici grubundaki her bölümün yalnızca bir etkin okuyucuya sahip olması mümkündür. Denetim noktası oluşturma, kiralama ve okuyucuları yönetme işlemleri, .NET istemcileri için [EventProcessorHost](/dotnet/api/microsoft.servicebus.messaging.eventprocessorhost) sınıfı kullanılarak basitleştirilir. Event Processor Host, akıllı bir tüketici aracısıdır.

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

Event Hubs işleme kapasitesi, *işleme birimleri* tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir işleme birimi aşağıdaki kapasiteyi içerir:

* Giriş: Saniyede 1 MB veya saniyede 1000 olaya kadar (hangisi önce gerçekleşirse)
* Çıkış: Saniyede 2 MB’ye kadar

Satın alınan işleme birimlerinin kapasitesi aşıldığında giriş azaltılır ve [ServerBusyException](/dotnet/api/microsoft.servicebus.messaging.serverbusyexception) döndürülür. Çıkış, azaltma özel durumları oluşturmaz, ancak yine de satın alınan işleme birimlerinin kapasitesiyle sınırlıdır. Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. Üretilen iş birimleri yönetebileceğiniz **ölçek** ad alanları dikey [Azure portal](https://portal.azure.com). Üretilen iş birimleri programlı olarak kullanarak da yönetebilirsiniz [olay hub'ları API'leri](event-hubs-api-overview.md).

İşleme birimleri saat başına faturalandırılır ve önceden satın alınır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. Bir Event Hubs ad alanı için en fazla 20 işleme birimi satın alınabilir ve ad alanındaki tüm Olay Hub’larında paylaşılır.

Azure desteğine başvurularak 20’li bloklar halinde olacak şekilde 100’e kadar daha fazla işleme birimi satın alınabilir. Bundan sonraki miktarlar için 100 işleme biriminden oluşan bloklar satın alabilirsiniz.

İşleme birimleri ile bölümleri en iyi ölçeği elde etmek için Bakiye öneririz. Tek bir bölüm en fazla bir işleme biriminden oluşan ölçeğe sahiptir. İşleme birimlerinin sayısı bir olay hub’ındaki bölüm sayısına eşit veya daha az olmalıdır.

Event Hubs ayrıntılı fiyatlandırma bilgileri için bkz. [Event Hubs fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

## <a name="next-steps"></a>Sonraki adımlar

Event Hubs hakkında daha fazla bilgi için şu bağlantıları ziyaret edin:

* [Event Hubs öğreticisi][Event Hubs tutorial] ile çalışmaya başlama
* [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md)
* [Event Hubs’da kullanılabilirlik ve tutarlılık](event-hubs-availability-and-consistency.md)
* [Event Hubs ile ilgili SSS](event-hubs-faq.md)
* [Olay hub'ları örnekleri][]

[Event Hubs tutorial]: event-hubs-dotnet-standard-getstarted-send.md
[Olay hub'ları örnekleri]: https://github.com/Azure/azure-event-hubs/tree/master/samples
