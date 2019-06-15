---
title: 'Azure Cosmos DB: .NET değişiklik akışı işlemci API, SDK ve kaynakları'
description: Değişiklik akışı işlemci API ve yayın tarihleri, sona erme tarihlerini ve her .NET değişiklik akışı işlemci SDK sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi alın.
author: ealsur
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 01/30/2019
ms.author: maquaran
ms.openlocfilehash: 2a4d636ccb03e36f7c495f3c10c90033d7c3c93c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66417905"
---
# <a name="net-change-feed-processor-sdk-download-and-release-notes"></a>İşlemci SDK'sı .NET değişiklik akışı: İndirme ve sürüm notları

> [!div class="op_single_selector"]
>
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik akışı](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](sql-api-query-reference.md)
> * [Bulkexecutor'a - .NET](sql-api-sdk-bulk-executor-dot-net.md)
> * [Bulkexecutor'a - Java](sql-api-sdk-bulk-executor-java.md)

|   |   |
|---|---|
|**SDK'sını indirme**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB.ChangeFeedProcessor/)|
|**API belgeleri**|[Değişiklik akışı işlemci kitaplığı API başvuru belgeleri](/dotnet/api/microsoft.azure.documents.changefeedprocessor?view=azure-dotnet)|
|**Kullanmaya başlama**|[Değişiklik akışı işlemci .NET SDK ile çalışmaya başlama](change-feed.md)|
|**Geçerli desteklenen çerçevesi**| [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</br> [Microsoft .NET Core](https://www.microsoft.com/net/download/core) |

## <a name="release-notes"></a>Sürüm notları

### <a name="v2-builds"></a>v2 oluşturur.

### <a name="a-name227227"></a><a name="2.2.7"/>2.2.7
* Senaryo için strateji tüm kiraları alırken geliştirilmiş yük dengelemeyi kira sona erme aralığını, örneğin ağ sorunları nedeniyle uzun sürer:
  * Bu senaryoda yük çalarak kiraları etkin sahipleri neden Dengeleme algoritması yanlışlıkla kiralama süresi dolmuş olarak dikkate alınması gereken kullanılır. Bu, gereksiz kiraları birçok yeniden Dengeleme tetikleyebilir.
  * Bu sorunu hangi sahibi değişmediğinden süresi dolmuş kiralama gerçekleştirmesi ve posponing alınırken sonraki Yük Dengeleme yineleme için kira süresi dolmuş olsa yeniden deneme sırasında çakışma önleyerek bu sürümde giderilen.

### <a name="a-name226226"></a><a name="2.2.6"/>2.2.6
* Gözlemci özel durumları işleme İyileştirildi.
* Gözlemci hatalarıyla ilgili daha zengin bilgi:
  * Gözlemci gözlemci'nın ProcessChangesAsync tarafından oluşturulan bir özel durum nedeniyle kapalı olduğunda, CloseAsync artık ayarlamak için ChangeFeedObserverCloseReason.ObserverError neden parametresi alır.
  * Kullanıcı kodunda bir gözlemci içindeki hataları belirlemek için eklenen izler.

### <a name="a-name225225"></a><a name="2.2.5"/>2.2.5
* Paylaşılan veritabanı aktarım hızını kullanan koleksiyonlardaki bölünmüş işlemek için destek eklendi.
  * Bu sürüm ne zaman sonucu oluşturulan, iki yerine yalnızca bir alt bölüm anahtar aralığı ile yeniden Dengeleme bölümü Böl paylaşılan veritabanı aktarım hızını kullanarak koleksiyonlardaki bölünmüş sırasında oluşabilecek bir sorunu giderir. Bu durumda, değişiklik akışı işlemci eski bölüm anahtar aralığı için Kiranın silinmesi ve yeni kiraları oluşturmama takılabilir. Bu sürümde olan sorun çözüldüğünde.

### <a name="a-name224224"></a><a name="2.2.4"/>2.2.4
* Eklenen yeni özellik başlangıç değişikliğini destekleyecek şekilde ChangeFeedProcessorOptions.StartContinuation isteği devamlılık belirteci akış. Bu, kira koleksiyonu boş olduğunda veya bir kira ContinuationToken kümesi yok. yalnızca kullanılır. Ayarlama ContinuationToken sahip kira kira koleksiyondaki ContinuationToken kullanılır ve ChangeFeedProcessorOptions.StartContinuation göz ardı edilir.

### <a name="a-name223223"></a><a name="2.2.3"/>2.2.3
* Bölüm başına devamlılık belirteçleri kalıcı hale getirmek için özel depo kullanma desteği eklendi.
  * Örneğin, bir özel kiralama deposu, Azure Cosmos DB kira koleksiyonu özel herhangi bir yolla bölümlenmiş olabilir.
  * Yeni genişletilebilirlik noktası ChangeFeedProcessorBuilder.WithLeaseStoreManager(ILeaseStoreManager) ve ortak arabirim ILeaseStoreManager özel kira depoları kullanabilirsiniz.
  * ILeaseManager arabirimi yeniden düzenlenen içinde birden çok rol arabirimi.
* Küçük değişiklik: kaldırılan genişletilebilirlik ChangeFeedProcessorBuilder.WithLeaseManager(ILeaseManager) noktası, ChangeFeedProcessorBuilder.WithLeaseStoreManager(ILeaseStoreManager) kullanın.

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2
* Bu sürüm, izlenen koleksiyonundaki bir bölünmüş işleme ve bölümlenmiş kira koleksiyonu kullanarak sırasında oluşan bir hatayı düzeltir. Bölünmüş bölüm için bir kira işlerken, bu bölüme karşılık gelen kira silinemez. Bu sürümde olan sorun çözüldüğünde.

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1
* Sabit Estimator hesaplama birden çok ana hesap ve yeni bir oturum belirteci biçimi.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0
* Bölümlenmiş kira koleksiyonları için destek eklendi. Bölüm anahtarı /id tanımlanması gerekir.
* Küçük değişiklik: IChangeFeedDocumentClient arabirimi ve ChangeFeedDocumentClient sınıfı yöntemlerinin parametreleri RequestOptions ve CancellationToken içerecek şekilde değiştirildi. IChangeFeedDocumentClient noktasıdır DocumentClient tasarlamanız ve ona ek izleme yapmak için tüm çağrıları ıntercept örn değişiklik akışı işlemci ile kullanmak için belge istemcisinin özel uygulanışı sağlamak izin veren bir gelişmiş Genişletilebilirlik hata işleme , vs. Bu güncelleştirme ile IChangeFeedDocumentClient uygulayan kodu uygulamasında yeni parametreleri içerecek şekilde değiştirilmesi gerekir.
* Küçük tanılama geliştirmeleri.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0
* Yeni API ile görev eklenen&lt;IReadOnlyList&lt;RemainingPartitionWork&gt; &gt; IRemainingWorkEstimator.GetEstimatedRemainingWorkPerPartitionAsync(). Bu, her bölüm için tahmin edilen iş almak için kullanılabilir.
* Microsoft.Azure.DocumentDB SDK 2.0 destekler. Microsoft.Azure.DocumentDB 2.0 veya sonraki sürümünü gerektirir.

### <a name="a-name206206"></a><a name="2.0.6"/>2.0.6
* Eklenen ChangeFeedEventHost.HostName ortak özellik v1 ile uyumluluk için.

### <a name="a-name205205"></a><a name="2.0.5"/>2.0.5
* Bölümü Böl sırasında oluşan bir yarış durumu düzeltildi. Yarış durumu kira alınıyor ve hemen sırasında bölüm bölünmüş kaybetme ve Çekişme neden neden olabilir. Bu sürümle birlikte yarış durumu sorun çözüldüğünde.

### <a name="a-name204204"></a><a name="2.0.4"/>2.0.4
* GA SDK'SI

### <a name="a-name203-prerelease203-prerelease"></a><a name="2.0.3-prerelease"/>2.0.3-prerelease
* Aşağıdaki sorunlar çözülmüştür:
  * Bölümü Böl gerçekleştiğinde olabilir bölme önce değiştirilen belgeler işlenmesi yinelenen.
  * Hiçbir kiralama kira koleksiyonda mevcut olduğunda GetEstimatedRemainingWork API 0 döndürdü.

* Aşağıdaki özel durumlar genel hale getirilir. IPartitionProcessor uygulayan uzantıları, bu özel durumlar.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.LeaseLostException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionException. 
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionNotFoundException.
  * Microsoft.Azure.Documents.ChangeFeedProcessor.Exceptions.PartitionSplitException. 

### <a name="a-name202-prerelease202-prerelease"></a><a name="2.0.2-prerelease"/>2.0.2-prerelease
* Küçük API değişiklikler:
  * Eski olarak işaretlendi ChangeFeedProcessorOptions.IsAutoCheckpointEnabled kaldırıldı.

### <a name="a-name201-prerelease201-prerelease"></a><a name="2.0.1-prerelease"/>2.0.1-prerelease
* Kararlılık geliştirmeleri:
  * Daha iyi işleme kira deposu başlatma. Kira deposu boş olduğunda, bir işlemci örneği başlatmak yalnızca diğer bekler.
  * Daha fazla stable/verimli kira yenileme/yayın. Yenileme ve bir kira bir bölüm bırakarak diğerleri yenilemesi bağımsızdır. V1'de, tüm bölümler için sıralı olarak yapılmadı.
* Yeni v2 API'si:
  * İşlemci Esnek yapımı için oluşturucu deseni: ChangeFeedProcessorBuilder sınıfı.
    * Parametreleri herhangi bir birleşimini gerçekleştirebilir.
    * DocumentClient örneği için izleme ve/veya kira koleksiyonu (v1'de kullanılamaz) alabilir.
  * IChangeFeedObserver.ProcessChangesAsync artık CancellationToken alıyor.
  * IRemainingWorkEstimator - kalan iş estimator işlemciden ayrı olarak kullanılabilir.
  * Yeni genişletilebilirlik noktaları:
    * IPartitionLoadBalancingStrategy - işlemci örnekleri arasında bölümlerinin özel Yük Dengeleme için.
    * ILease, ILeaseManager - özel kiralama Yönetimi'ni tıklatın.
    * IPartitionProcessor - bir bölüme değişiklikleri özel işleme için.
* Oturum açma - kullanan [LibLog](https://github.com/damianh/LibLog) kitaplığı.
* % 100 API v1 ile geriye dönük uyumlu.
* Yeni kod tabanını.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) 1.21.1 sürümleri ve üzeri.

### <a name="v1-builds"></a>V1 oluşturur

### <a name="a-name133133"></a><a name="1.3.3"/>1.3.3
* Daha fazla günlük eklendi.
* Bekleyen iş tahmini birden çok kez çağrılırken bir DocumentClient sızıntısı düzeltildi.

### <a name="a-name132132"></a><a name="1.3.2"/>1.3.2
* Bekleyen iş tahmini düzeltmeler.

### <a name="a-name131131"></a><a name="1.3.1"/>1.3.1
* Kararlılık geliştirmeleri.
  * İşlemek için yol açabilecek iptal edilen görevler sorunu için düzeltme gözlemciler bazı bölümler durduruldu.
* El ile denetim noktası oluşturma desteği.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) 1.21 sürümleri ve üzeri.

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* .NET Standard 2.0 için destek ekler. Paket artık destekliyor `netstandard2.0` ve `net451` framework adlar.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) 1.17.0 sürümleri ve üzeri.
* Uyumlu [SQL .NET Core SDK'sı](sql-api-sdk-dotnet-core.md) 1.5.1 sürümleri ve üzeri.

### <a name="a-name111111"></a><a name="1.1.1"/>1.1.1
* Bir sorun, değişiklik akışı boş veya hiçbir iş bekleyen kalan çalışma tahmini hesaplanması düzeltir.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) 1.13.2 sürümleri ve üzeri.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Değişiklik akışı işlenecek kalan çalışma tahmini elde etmek için bir yöntem eklendi.
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) 1.13.2 sürümleri ve üzeri.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK'SI
* Uyumlu [SQL .NET SDK'sı](sql-api-sdk-dotnet.md) sürümleri 1.14.1 ve altı.

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri

Sağlar; Microsoft bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

Geçerli SDK'sı yalnızca eklenen yeni özellikler ve işlevsellik ve en iyi duruma getirme, bu nedenle, her zaman en son SDK sürümüne erken mümkün olduğunca yükseltmeniz önerilir. 

Cosmos DB devre dışı bırakılan bir SDK'sını kullanarak yapılan tüm istekleri hizmet tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.2.7](#2.2.7) |14 Mayıs 2019 |--- |
| [2.2.6](#2.2.6) |29 Ocak 2019 |--- |
| [2.2.5](#2.2.5) |13 Aralık 2018'e |--- |
| [2.2.4](#2.2.4) |29 Kasım 2018 |--- |
| [2.2.3](#2.2.3) |19 Kasım 2018 |--- |
| [2.2.2](#2.2.2) |31 Ekim 2018 |--- |
| [2.2.1](#2.2.1) |24 Ekim 2018 |--- |
| [1.3.3](#1.3.3) |08 Mayıs 2018 |--- |
| [1.3.2](#1.3.2) |18 Nisan 2018 |--- |
| [1.3.1](#1.3.1) |13 Mart 2018 |--- |
| [1.2.0](#1.2.0) |31 Ekim 2017 |--- |
| [1.1.1](#1.1.1) |29 Ağustos 2017 |--- |
| [1.1.0](#1.1.0) |13 Ağustos 2017 |--- |
| [1.0.0](#1.0.0) |07 Temmuz 2017 |--- |

## <a name="faq"></a>SSS

[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.

Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası.
