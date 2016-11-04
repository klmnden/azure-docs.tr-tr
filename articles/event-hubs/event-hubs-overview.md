---
title: Azure Event Hubs’a Genel Bakış | Microsoft Docs
description: Azure Event Hubs’a giriş ve genel bakış.
services: event-hubs
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: event-hubs
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/16/2016
ms.author: sethm

---
# <a name="azure-event-hubs-overview"></a>Azure Event Hubs’a genel bakış
Birçok modern çözüm uyarlamalı müşteri deneyimleri sağlamayı veya sürekli geri bildirim ve otomatik telemetri ile ürünleri iyileştirmeyi amaçlar. Bu tür çözümler çok sayıda eş zamanlı yayımcıdan çok büyük miktarda bilgiyi güvenli ve güvenilir bir şekilde işleme zorluğuyla karşı karşıya kalmaktadır. Microsoft Azure Event Hubs çok çeşitli senaryolarda büyük ölçekli veri girişi için temel sağlayan, yönetilen bir platformdur. Bu tür senaryoların örnekleri mobil uygulamalarda davranış işleme, web gruplarından trafik bilgileri, konsol oyunlarında oyun içi olay yakalama veya sanayi makinelerinden ya da bağlı taşıtlardan toplanan telemetri verileridir. Event Hubs’ın çözüm mimarilerinde oynadığı genel rol, bir olay ardışık düzeni için "ön kapı" olarak görev yapmalarıdır ve çoğunlukla *olay yutucu* olarak adlandırılırlar. Olay yutucu, bir olay akışının üretimini ilgili olayların kullanılmasına ayıran, olay yayımcıları ile olay tüketicileri arasında duran bir bileşen veya hizmettir.

![Event Hubs](./media/event-hubs-overview/IC759856.png)

Azure Event Hubs, düşük gecikme süresi ve yüksek güvenilirlikle bulutta büyük ölçekte telemetri girişi sağlayan bir olay işleme hizmetidir. Bu hizmet, diğer aşağı akış hizmetleriyle birlikte kullanıldığında özellikle uygulama izleme, kullanıcı deneyiminde veya iş akışı işlemede ve Nesnelerin İnterneti (IoT) senaryolarında yararlıdır. Event Hubs bir ileti akışı işleme yeteneği sağlar ve bir Event Hub'ı kuyruklara ve konulara benzer bir varlık olsa da geleneksel kurumsal mesajlaşmadan çok farklı olan özelliklere sahiptir. Kurumsal mesajlaşma senaryoları için genellikle sıralama, ulaşmayan posta, işlem desteği ve güçlü teslim güvenceleri gibi gelişmiş özellikler gerekirken olay girişi konusunda öncelikle, olay akışlarında yüksek verimlilik ve işleme esnekliğine odaklanılır. Bu nedenle, Event Hubs özellikleri yüksek verimlilik ve olay işleme senaryolarına doğru güçlü eğilimi nedeniyle Service Bus konularından farklıdır. Bu nedenle, Event Hubs konular için mevcut olan bazı mesajlaşma özelliklerini kullanılmaz. Bu özellikler gerekli olursa hala en uygun seçenek konulardır.

Olay Hub'ı, Service Bus kuyrukları ve konularına benzer şekilde Service Bus ad alanı düzeyinde oluşturulur. Event Hubs birincil API arabirimleri olarak AMQP ve HTTP kullanır. Aşağıdaki diyagramda Event Hubs ile Service Bus arasındaki ilişki gösterilmektedir.

![Event Hubs](./media/event-hubs-overview/IC741188.png)

## <a name="conceptual-overview"></a>Kavramsal genel bakış
Event Hubs bölünmüş bir tüketici modeli aracılığıyla ileti akışı sağlar. Kuyruklar ve konular her bir tüketicinin aynı kuyruk veya kaynaktan okumaya çalıştığı [Yarışan Tüketici](https://msdn.microsoft.com/library/dn568101.aspx) modelini kullanır. Kaynakların bu rekabeti sonuç olarak akış işleme uygulamaları için karmaşıklık ve ölçek sınırlamaları ile sonuçlanır. Event Hubs her bir tüketicinin ileti akışında yalnızca belirli bir alt küme ya da bölümü okuduğu bölünmüş bir tüketici modeli kullanır. Bu model, olay işleme için yatay ölçek sağlar ve kuyruklar ile konularda kullanılamayan diğer akış odaklı özellikleri sunar.

### <a name="partitions"></a>Bölümler
Bölüm bir Event Hub'ında tutulan olayların sıralı dizisidir. Yeni olaylar geldikçe dizinin sonuna eklenir. Bölüm bir "yürütme günlüğü" olarak düşünülebilir.

![Event Hubs](./media/event-hubs-overview/IC759857.png)

Bölümler Event Hub'ı düzeyinde ayarlanan yapılandırılmış bir elde tutma süresi boyunca verileri saklar. Bu ayar Event Hub'ındaki tüm bölümler için geçerlidir. Olayların süresi saat bazında dolar; bunları açıkça silemezsiniz. Event Hub'ı birden çok bölüm içerir. Her bölüm bağımsızdır ve kendi veri dizisini içerir. Sonuç olarak bölümler genellikle farklı hızlarda artar.

![Event Hubs](./media/event-hubs-overview/IC759858.png)

Bölüm sayısı, Event Hub'ı oluşturma zamanında belirtilir ve 2 ile 32 arasında olmalıdır (varsayılan 4'tür). Bölümler bir veri düzenleme mekanizmasıdır ve Event Hubs işlemesinden daha çok, tüketen uygulamalarda gerekli aşağı akış paralellik derecesiyle ilgilidir. Bu durum bir Event Hub'ındaki bölüm sayısı seçimini, sahip olmayı beklediğiniz eşzamanlı okuyucu sayısıyla doğrudan ilgili hale getirir. Event Hub'ı oluşturulduktan sonra bölüm sayısı değiştirilemez; bu sayıyı uzun vadeli beklenen ölçek açısından göz önünde bulundurmanız gerekir. 32 olan bölüm sınırını Service Bus ekibine başvurarak artırabilirsiniz.

Bölümler, tanımlanabilir ve doğrudan gönderilebilir olsa da belirli bölümlere veri göndermekten kaçınmanız en iyisidir. Bunun yerine, [Olay yayımcısı](#event-publisher) ve [Yayımcı İlkesi](#capacity-and-security) bölümlerinde sunulan daha yüksek düzeyli yapıları kullanabilirsiniz.

Event Hubs bağlamında iletiler *olay verileri* olarak adlandırılır. Olay verileri olayın gövdesini, kullanıcı tarafından tanımlanan bir özellik paketini ve bölümdeki sapması ve akış dizisindeki sayısı gibi olaya ilişkin çeşitli meta verileri içerir. Bölümler bir olay verileri dizisi ile doldurulur.

## <a name="event-publisher"></a>Olay yayımcısı
Bir Event Hub'ına olayları ya da verileri gönderen herhangi bir varlık *Olay Yayımcısı* ’dır. Olay yayımcıları olayları HTTPS veya AMQP 1.0 kullanarak yayımlayabilir. Olay yayımcıları kendilerini bir Event Hub'ına tanıtmak için Paylaşılan Erişim İmzası (SAS) belirteci kullanır ve senaryonun gereksinimlerine bağlı olarak benzersiz bir kimliğe sahip olabilir ya da ortak bir SAS belirteci kullanabilir.

SAS ile çalışma hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus/service-bus-shared-access-signature-authentication.md).

### <a name="common-publisher-tasks"></a>Ortak yayımcı görevleri
Bu bölümde olay yayımcıları için ortak görevler açıklanmaktadır.

#### <a name="acquire-a-sas-token"></a>SAS belirteci alma
Paylaşılan Erişim İmzası (SAS), Event Hubs için kimlik doğrulama mekanizmasıdır. Service Bus, ad alanı ve Event Hub'ı düzeyinde SAS ilkeleri sağlar. SAS belirteci bir SAS anahtarından oluşturulur ve belirli bir biçimde kodlanmış bir URL’nin SHA karmasıdır. Service Bus anahtar (ilke) ve belirtecin adını kullanarak karmayı yeniden oluşturabilir ve böylece gönderenin kimliğini doğrular. Normalde, olay yayımcıları için SAS belirteci yalnızca belirli bir Event Hub'ı üzerindeki **gönder** ayrıcalıkları ile oluşturulur. Bu SAS belirteci URL mekanizması, yayımcı ilkesinde sunulan yayımcı kimliğinin temelini oluşturur. SAS ile çalışma hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus/service-bus-shared-access-signature-authentication.md).

#### <a name="publishing-an-event"></a>Olay yayımlama
Bir olayı AMQP 1.0 veya HTTPS üzerinden yayımlayabilirsiniz. Service Bus, .NET istemcilerinden bir Event Hub'ına olayları yayımlamak için [EventHubClient](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventhubclient.aspx) sınıfını sağlar. Diğer çalışma zamanları ve platformlar için [Apache Qpid](http://qpid.apache.org/) gibi herhangi bir AMQP 1.0 istemcisi kullanabilirsiniz. Olayları ayrı ayrı veya toplu olarak yayımlayabilirsiniz. Tek bir yayın (olay verileri örneği), tek bir olay ya da toplu işlem olmasına bakılmaksızın 256KB sınırlamaya sahiptir. Bundan büyük olayların yayımlanması bir hatayla sonuçlanır. Yayımcıların Event Hub'ındaki bölümleri bilmemesi ve yalnızca bir *bölüm anahtarı* (sonraki bölümde açıklanmıştır) ya da kimliklerini SAS belirteci üzerinden belirtmeleri en iyi yöntemdir.

AMQP veya HTTPS kullanma seçimi kullanım senaryosuna bağlıdır. AMQP, taşıma düzeyi güvenliği (TLS) veya SSL/TLS’ye ek olarak kalıcı bir çift yönlü yuva oluşturulmasını gerektirir. Bu işlem ağ trafiği açısından pahalı olabilir, ancak yalnızca bir AMQP oturumunun başlangıcında gerçekleştirilir. HTTPS daha düşük bir başlangıç yüküne sahiptir, ancak her istek için ek SSL yükü gerektirir. Sıklıkla olay yayımlayan yayımcılar için AMQP önemli performans, gecikme ve verimlilik tasarrufları sağlar.

### <a name="partition-key"></a>Bölüm anahtarı
Bölüm anahtarı gelen olay verilerini veri düzenleme amacıyla belirli bölümlere eşlemek için kullanılan bir değerdir. Bölüm anahtarı, gönderen tarafından belirtilip bir Event Hub'ına geçirilen değerdir. Statik karma işlevi ile işlenir ve sonuçta bölüm ataması oluşturulur. Bir olayı yayımlarken bölüm anahtarı belirtmezseniz hepsini bir kez deneme ataması kullanılır. Bölüm anahtarlarını kullanırken olay yayımcısı yalnızca bölüm anahtarını bilir, olayların yayımlandığı bölümü bilmez. Anahtar ile bölümün bu şekilde ayrılması göndereni aşağı akış işleme ve olay depolama hakkında çok fazla bilgi sahibi olma gereksiniminden kurtarır. Bölüm anahtarları aşağı akış işleme için veri düzenlemede önemlidir, ancak temelde bölümlerin kendisiyle ilişkili değildir. Cihaz veya kullanıcı başına benzersiz bir kimlik iyi bir bölüm anahtarı oluşturur, ancak ilgili olayları tek bir bölümde gruplandırmak için coğrafi bölge gibi diğer öznitelikler de kullanılabilir. Aşağıdaki görüntüde bölümlere sabitlenecek bölüm anahtarlarını kullanan olay gönderenleri gösterilmektedir.

![Event Hubs](./media/event-hubs-overview/IC759859.png)

Event Hubs aynı bölüm anahtarı değerini paylaşan tüm olayların aynı sırayla ve aynı bölüme iletilmesini sağlar. Önemli olan husus, bölüm anahtarlarının sonraki bölümde açıklanan yayımcı ilkeleriyle birlikte kullanılması durumunda yayım kimliğinin ve bölüm anahtarı değerinin eşleşmesi gerektiğidir. Aksi takdirde bir hata oluşur.

### <a name="event-consumer"></a>Olay tüketicisi
Bir Event Hub'ından olay verilerini okuyan herhangi bir varlık olay tüketicisidir. Tüm olay tüketicileri bir tüketici grubundaki bölümler üzerinden olay akışını okur. Her bölüm aynı anda yalnızca bir etkin okuyucuya sahip olmalıdır. Tüm Event Hubs tüketicileri, olayların kullanılabilir olduğu anda iletildiği AMQP 1.0 oturumu üzerinden bağlanır. İstemcinin veri kullanılabilirliğini yoklaması gerekmez.

#### <a name="consumer-groups"></a>Tüketici grupları
Event Hubs yayımlama/abonelik mekanizması tüketici grupları aracılığıyla etkinleştirilir. Tüketici grubu tüm Event Hub'ının bir görünümüdür (durum, konum veya uzaklık). Tüketici grupları birden çok tüketen uygulamayı her biri olay akışının ayrı bir görünümüne sahip olacak ve akışı kendi hızlarında ve kendi sapmalarıyla bağımsız bir şekilde okuyacak şekilde etkinleştirir. Bir akış işleme mimarisinde her bir aşağı akış uygulaması bir tüketici grubuna karşılık gelir. Olay verilerini uzun süreli depolama alanına yazmak isterseniz bu depolama yazma uygulaması bir tüketici grubudur. Karmaşık olay işlemesi ise başka ve ayrı bir tüketici grubu tarafından gerçekleştirilir. Bölümlere yalnızca bir tüketici grubu üzerinden erişebilirsiniz. Bir Event Hub'ında her zaman varsayılan bir tüketici grubu vardır ve Standart katmanlı bir Event Hub'ı için en fazla 20 tüketici grubu oluşturabilirsiniz.

Tüketici grubu URI kuralının örnekleri aşağıda verilmiştir:

    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #1>
    //<my namespace>.servicebus.windows.net/<event hub name>/<Consumer Group #2>

Aşağıdaki görüntüde tüketici gruplarındaki olay tüketicileri gösterilmiştir.

![Event Hubs](./media/event-hubs-overview/IC759860.png)

#### <a name="stream-offsets"></a>Akış uzaklıkları
Uzaklık, bir olayın bölüm içindeki konumudur. Uzaklığı istemci tarafındaki bir imleç olarak düşünebilirsiniz. Uzaklık, olayın bayt cinsinden numaralandırılmasıdır. Bu değer, olay tüketicisinin (okuyucu) olay akışında olayları okumaya başlamak istediği bir noktayı belirtmesini sağlar. Uzaklığı bir zaman damgası veya bir uzaklık değeri olarak belirtebilirsiniz. Tüketiciler, kendi uzaklık değerlerini Event Hubs hizmetinin dışında saklamaktan sorumludur.

![Event Hubs](./media/event-hubs-overview/IC759861.png)

Bir bölüm içinde her olay bir uzaklık içerir. Bu uzaklık belli bir bölüm için olay dizisindeki konumu göstermek üzere tüketiciler tarafından kullanılır. Okuyucu bağlandığında uzaklıklar Event Hub'ına bir sayı veya zaman damgası değeri olarak geçirilebilir.

#### <a name="checkpointing"></a>Denetim noktası oluşturma
*Denetim noktası oluşturma*, okuyucuların bir bölüm olay dizisindeki konumlarını işaretledikleri veya uyguladıkları bir işlemdir. Denetim noktası oluşturma, tüketicinin sorumluluğundadır ve bir tüketici grubunda bölüm başına temelinde gerçekleşir. Diğer bir deyişle, her bir tüketici grubu için her bölüm okuyucusu geçerli konumunu olay akışında izlemelidir ve veri akışının tamamlandığını düşündüğünde hizmeti bilgilendirebilir. Bir okuyucunun bölüm bağlantısı kesilirse yeniden bağlandığında ilgili tüketici grubundaki o bölümün son okuyucusu tarafından daha önce gönderilen denetim noktasında okumaya başlar. Okuyucu bağlandığında okumaya başlayacağı konumu belirtmek üzere bu uzaklığı Event Hub'ına geçirir. Bu şekilde denetim noktası oluşturma özelliğini hem aşağı akış uygulamaları ile olayları "tamamlandı" olarak işaretlemek hem de farklı makinelerde çalışan okuyucular arasında bir yük devretme oluşması durumunda esneklik sağlamak amacıyla kullanabilirsiniz. Olay verileri Event Hub'ının oluşturulduğu sırada belirtilen elde tutma aralığı boyunca saklandığı için, bu denetim noktası oluşturma işleminden daha düşük bir uzaklık belirterek daha eski verilere geri dönülebilir. Bu mekanizmayla denetim noktası oluşturma özelliği hem yük devretme esnekliği hem de denetimli olay akışı yeniden yürütmesi sağlar.

#### <a name="common-consumer-tasks"></a>Ortak tüketici görevleri
Bu bölümde Event Hubs olay tüketicilerine veya okuyucularına yönelik ortak görevler açıklanmaktadır. Tüm Event Hubs tüketicileri AMQP 1.0 üzerinden bağlanır. AMQP 1.0 bir oturum ve durumu algılayan çift yönlü iletişim kanalıdır. Her bölümde bölüme göre ayrılmış olayların taşınmasını kolaylaştıran bir AMQP 1.0 bağlantı oturumu vardır.

##### <a name="connect-to-a-partition"></a>Bir bölüme bağlanma
Event Hub'ındaki olayları kullanmak için bir tüketicinin bölüme bağlanması gerekir. Daha önce belirtildiği gibi bölümlere her zaman bir tüketici grubu aracılığıyla erişirsiniz. Bölümlenmiş tüketici modelinin bir parçası olarak bölüm üzerinde aynı anda bir tüketici grubundan yalnızca tek bir okuyucu etkin olmalıdır. Bölümlere doğrudan bağlanırken okuyucu bağlantılarının belirli bölümlerle koordine edilmesi için bir kiralama mekanizmasının kullanılması yaygın bir uygulamadır. Bu şekilde, bir tüketici grubundaki her bölümün yalnızca bir etkin okuyucuya sahip olması mümkündür. Bir okuyucunun dizisindeki konumun yönetilmesi, denetim noktası oluşturma özelliğiyle gerçekleştirilen önemli bir görevdir. Bu işlev .NET istemcileri için [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) sınıfı kullanılarak basitleştirilir. [EventProcessorHost](https://msdn.microsoft.com/library/microsoft.servicebus.messaging.eventprocessorhost.aspx) akıllı bir tüketici aracısıdır ve sonraki bölümde açıklanmıştır.

##### <a name="read-events"></a>Olayları okuma
Belirli bir bölüm için bir AMQP 1.0 oturumu ve bağlantı açıldıktan sonra olaylar Event Hubs hizmeti tarafından AMQP 1.0 istemcisine teslim edilir. Bu teslim mekanizması, HTTP GET gibi çekme tabanlı mekanizmalardan daha yüksek verimlilik ve daha düşük gecikme sağlar. Olaylar istemciye gönderildiğinde her bir olay verisi örneği, olay dizisinde denetim noktası oluşturmayı kolaylaştırmak için kullanılan uzaklık ve dizi numarası gibi önemli meta veriler içerir.

![Event Hubs](./media/event-hubs-overview/IC759862.png)

Bu uzaklığı, akış işlemede ilerleyişi yönetmenizi en iyi şekilde sağlayan bir yöntem kullanarak yönetmek sizin sorumluluğunuzdadır.

## <a name="capacity-and-security"></a>Kapasite ve güvenlik
Event Hubs, akış girişi için yüksek oranda ölçeklenebilen paralel bir mimaridir. Bu nedenle, bir çözümün Event Hubs temel alınarak boyutlandırılması ve ölçeklendirilmesi sırasında dikkate alınması gereken birkaç temel unsur vardır. Bu kapasite denetimlerinin birincisi, aşağıdaki bölümde açıklanan *işleme birimleridir*.

### <a name="throughput-units"></a>İşleme birimleri
Event Hubs işleme kapasitesi, işleme birimleri tarafından denetlenir. İşleme birimleri önceden satın alınan kapasite birimleridir. Tek bir işleme birimi aşağıdakileri içerir:

* Giriş: Saniye başına 1 MB’a veya saniye başına 1000 olaya kadar.
* Çıkış: Saniye başına 2 MB’a kadar.

Giriş, satın alınan işleme birimi sayısına göre sağlanan kapasite miktarı ile kısıtlanır. Bu miktarın üzerinde veri gönderilmesi bir "kota aşıldı" özel durumu ile sonuçlanır. Bu miktar saniyede 1 MB veya saniyede 1000 olaydır (hangisi önce gerçekleşirse). Çıkış azaltma özel durumları oluşturmaz, ancak satın alınan işleme birimlerine göre sağlanan veri aktarımı miktarı ile sınırlıdır (bir işleme birimi için saniyede 2 MB). Yayımlama hızı özel durumları alırsanız veya daha yüksek çıkış görmeyi bekliyorsanız Event Hub'ının oluşturulduğu ad alanı için kaç tane işleme birimi satın aldığınızı denetlediğinizden emin olun. Daha fazla işleme birimi edinmek için [Klasik Azure portalı][Klasik Azure portalı]’nın **Ölçek** sekmesinde bulunan **Ad alanları** sayfasındaki ayarı düzenleyebilirsiniz. Ayrıca Azure API'lerini kullanarak bu ayarı değiştirebilirsiniz.

Bölümler bir veri düzenleme kavramıyken, işleme birimleri tamamen kapasiteyle ilgili bir kavramdır. İşleme birimleri saat başına faturalandırılır ve önceden satın alınır. Satın alındıktan sonra işleme birimleri en az bir saat için faturalandırılır. Bir Event Hubs ad alanı için en fazla 20 işleme birimi satın alınabilir ve bir Azure hesabının sınırı 20 işleme birimidir. Bu işleme birimleri belirli bir ad alanındaki tüm Event Hubs arasında paylaşılır.

İşleme birimleri en iyi çaba ilkesine göre sağlanır ve her zaman hemen satın almak için uygun olmayabilir. Belirli bir kapasite gerekiyorsa bu işleme birimlerini önceden satın almanız önerilir. 20'den fazla işleme birimi gerekiyorsa ilk 100 işleme birimine kadar 20’li bloklar içeren bir taahhüt temelinde daha fazla işleme birimi satın almak üzere Azure desteğine başvurabilirsiniz. Bundan sonraki miktarlar için 100 işleme biriminden oluşan bloklar satın alabilirsiniz.

Event Hubs ile en iyi ölçeği elde etmek için işleme birimleri ve bölümlerini dikkatlice dengelemeniz önerilir. Tek bir bölüm en fazla bir işleme biriminden oluşan ölçeğe sahiptir. İşleme birimlerinin sayısı bir Event Hub’ındaki bölüm sayısına eşit veya daha az olmalıdır.

Ayrıntılı fiyatlandırma bilgileri için bkz. [Event Hubs Fiyatlandırması](https://azure.microsoft.com/pricing/details/event-hubs/).

### <a name="publisher-policy"></a>Yayımcı ilkesi
Event Hubs, *yayımcı ilkeleri* aracılığıyla olay yayımcıları üzerinde ayrıntılı denetim sağlar. Yayımcı ilkeleri çok sayıda bağımsız olay yayımcısını kolaylaştırmak için tasarlanmış bir çalışma zamanı özellikleri kümesidir. Yayımcı ilkeleriyle her yayımcı, olayları aşağıdaki mekanizmayı kullanarak bir Event Hub'ında yayımlarken kendi benzersiz tanımlayıcısını kullanır:

    //<my namespace>.servicebus.windows.net/<event hub name>/publishers/<my publisher name>

Yayımcı adlarını önceden oluşturmanız gerekli değildir, ancak bunlar bağımsız yayımcı kimlikleri sağlamak amacıyla bir olayı yayımlarken kullanılan SAS belirteci ile eşleşmelidir. SAS hakkında daha fazla bilgi için bkz. [Service Bus ile Paylaşılan Erişim İmzası Kimlik Doğrulaması](../service-bus/service-bus-shared-access-signature-authentication.md). Yayımcı ilkelerini kullanırken **PartitionKey** değeri yayımcı adına ayarlanır. Bu hizmetin düzgün çalışması için bu değerlerin eşleşmesi gerekir.

## <a name="summary"></a>Özet
Azure Event Hubs herhangi bir ölçekteki ortak uygulama ve kullanıcı iş akışı için kullanılabilen bir hiper ölçek olayı ve telemetri işlemesi sağlar. Düşük gecikme ile yoğun ölçekte yayımlama-abonelik özellikleri sağlayabilen Event Hubs, Büyük Veriler için "kestirme yol" olarak görev yapar. Yayımcıya dayalı kimlik ve iptal listeleri ile bu özellikler yaygın Nesnelerin İnterneti senaryolarına genişletilmektedir. Event Hubs uygulamaları geliştirme hakkında daha fazla bilgi için bkz. [Event Hubs programlama kılavuzu](event-hubs-programming-guide.md).

## <a name="next-steps"></a>Sonraki adımlar
Event Hubs kavramlarını öğrendiğinize göre aşağıdaki senaryolara geçebilirsiniz:

* [Event Hubs öğreticisi] ile çalışmaya başlama.
* [Event Hubs kullanan bir örnek uygulamanın] tamamı.

[Klasik Azure portalı]: http://manage.windowsazure.com
[Event Hubs öğreticisi]: event-hubs-csharp-ephcs-getstarted.md
[Event Hubs kullanan örnek uygulama]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Event-Hub-286fd097



<!--HONumber=Oct16_HO3-->


