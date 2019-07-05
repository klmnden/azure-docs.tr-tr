---
title: 'Azure Cosmos DB: SQL .NET API, SDK ve kaynakları'
description: Tüm SQL .NET API ve yayın tarihleri, sona erme tarihlerini ve her bir Azure Cosmos DB .NET SDK'sı sürümü arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
author: SnehaGunda
ms.service: cosmos-db
ms.subservice: cosmosdb-sql
ms.devlang: dotnet
ms.topic: reference
ms.date: 03/09/2018
ms.author: sngun
ms.openlocfilehash: 4f502984a09f81b5aaf0568c84b75832f8164151
ms.sourcegitcommit: 5bdd50e769a4d50ccb89e135cfd38b788ade594d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/03/2019
ms.locfileid: "67541040"
---
# <a name="azure-cosmos-db-net-sdk-for-sql-api-download-and-release-notes"></a>Azure Cosmos DB SQL API'si için .NET SDK: İndirme ve sürüm notları
> [!div class="op_single_selector"]
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

| |  |
|---|---|
|**SDK'sını indirme**|[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)|
|**API belgeleri**|[.NET API başvuru belgeleri](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)|
|**Örnekler**|[.NET kodu örnekleri](sql-api-dotnet-samples.md)|
|**Kullanmaya başlama**|[Azure Cosmos DB .NET SDK ile çalışmaya başlama](sql-api-get-started.md)|
|**Web uygulaması Öğreticisi**|[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md)|
|**Geçerli desteklenen çerçevesi**|[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)|

## <a name="release-notes"></a>Sürüm notları

### <a name="a-name3001-preview3001-preview"></a><a name="3.0.0.1-preview"/>3.0.0.1-Preview
* 1 önizlemesi [sürüm 3.0.0](https://www.nuget.org/packages/Microsoft.Azure.Cosmos/) genel önizlemesi için .NET SDK'sının.
* Hedef .NET framework 4.6.1+ .NET ve .NET Core 2.0 + destekleyen standardı
* Yeni nesne modeli, üst düzey CosmosClient ve yöntemleri ile ilgili CosmosDatabases ve CosmosContainers CosmosItems sınıflar arasında bölün. 
* Akışları için destek. 
* Durum kodu döndürür ve yanıt döndürüldüğünde yalnızca durum sunucudan CosmosResponseMessage güncelleştirildi. 

### <a name="a-name251251"></a><a name="2.5.1"/>2.5.1

* SDK'ın System.Net.Http sürümü artık NuGet paketinde tanımlanan eşleşir.
* Özgün biri başarısız olursa farklı bir bölgeye geri yazma isteklerine izin verin.
* Oturumu yeniden deneme ilkesi için yazma isteği ekleyin.

### <a name="a-name241241"></a><a name="2.4.1"/>2.4.1

* Düzeltmeleri boş sayfalar neden sorgular için yarış durumu izleme

### <a name="a-name240240"></a><a name="2.4.0"/>2.4.0

* LINQ sorguları için Ondalık Duyarlığı boyutu artar.
* Yeni eklenen sınıflarda CompositePath CompositePathSortOrder, SpatialSpec SpatialType ve PartitionKeyDefinitionVersion
* DocumentCollection için eklenen TimeToLivePropertyPath
* Eklenen CompositeIndexes ve SpatialIndexes IndexPolicy için
* Eklenen PartitionKeyDefinition sürümüne
* PartitionKey Yok'a eklendi

### <a name="a-name230230"></a><a name="2.3.0"/>2.3.0

 * Eklenen IdleTcpConnectionTimeout, OpenTcpConnectionTimeout, MaxRequestsPerTcpConnection ve MaxTcpConnectionsPerEndpoint ConnectionPolicy için.

### <a name="a-name223223"></a><a name="2.2.3"/>2.2.3

* Tanılama geliştirmeleri

### <a name="a-name222222"></a><a name="2.2.2"/>2.2.2

* Eklenen ortam değişkeni ayarını "POCOSerializationOnly".

* DocumentDB.Spatial.Sql.dll kaldırılır ve artık Microsoft.Azure.Documents.ServiceInterop.dll dahil

### <a name="a-name221221"></a><a name="2.2.1"/>2.2.1

* Yeniden deneme mantığı için StoredProcedure gelişme yük devretme sırasında çağrıları yürütün.

* Yapılan DocumentClientEventSource tekli. 

* ConnectionPolicy RequestTimeout uygularken değil GatewayAddressCache zaman aşımı düzeltin.

### <a name="a-name220220"></a><a name="2.2.0"/>2.2.0

* Doğrudan/TCP taşıma tanılama için TransportException, SDK'sının bir iç özel durum türü eklediniz. Bu tür, mevcut olduğunda özel durum iletileri, istemci bağlantısı sorunlarını gidermek için ek bilgi yazdırır.

* HttpClient istekleri (örn., HttpClientHandler) göndermek için kullanılacak HTTP işleyici yığını bir HttpMessageHandler alan eklenen yeni oluşturucu aşırı yüklemesi.

* Burada üstbilgisi null değerlerle düzgün bir şekilde işlenmemiş olan hata düzeltildi.

* Koleksiyon önbellek doğrulama geliştirildi.

### <a name="a-name213213"></a><a name="2.1.3"/>2.1.3

* Güncelleştirilmiş System.Net.Security 4.3.2 için.

### <a name="a-name212212"></a><a name="2.1.2"/>2.1.2'yi

* Tanılama izleme geliştirmeleri

### <a name="a-name211211"></a><a name="2.1.1"/>2.1.1

* Çok bölgeli isteği geçici hatalara karşı daha fazla esneklik eklenmiştir.

### <a name="a-name210210"></a><a name="2.1.0"/>2.1.0

* Eklenen çok bölgeli yazma desteği.
* Bölüm sorgu performansı geliştirmelerinin üst ve MaxBufferedItemCount çapraz.

### <a name="a-name200200"></a><a name="2.0.0"/>2.0.0

* Eklenen isteği iptal etme desteği.
* Eklenen SetCurrentLocation için form veya ConnectionPolicy tercih edilen konumlar bölgeye göre otomatik olarak doldurur.
* Çapraz bölüm Min/Maks ve sorgular tek bir bölüme belge eşleşen filtre hata düzeltildi.
* DocumentClient yöntemleri artık IDocumentClient ile eşlik vardır.
* Kurulan bağlantı sayısını azaltmak için güncelleştirilmiş doğrudan TCP taşıma yığını.
* Windows olmayan istemciler için eklenen destek doğrudan modu TCP için.

### <a name="a-name200-preview2200-preview2"></a><a name="2.0.0-preview2"/>2.0.0-preview2

* Eklenen isteği iptal etme desteği.
* Eklenen SetCurrentLocation için form veya ConnectionPolicy tercih edilen konumlar bölgeye göre otomatik olarak doldurur.
* Çapraz bölüm Min/Maks ve sorgular tek bir bölüme belge eşleşen filtre hata düzeltildi.

### <a name="a-name200-preview200-preview"></a><a name="2.0.0-preview"/>2.0.0-Preview

* DocumentClient yöntemleri artık IDocumentClient ile eşlik vardır.
* Kurulan bağlantı sayısını azaltmak için güncelleştirilmiş doğrudan TCP taşıma yığını.
* Windows olmayan istemciler için eklenen destek doğrudan modu TCP için.

### <a name="a-name12201220"></a><a name="1.22.0"/>1.22.0

* FeedOptions için ConsistencyLevel özelliği eklendi.
* Eklenen JsonSerializerSettings RequestOptions ve FeedOptions.
* Eklenen EnableReadRequestsFallback ConnectionPolicy için.

### <a name="a-name12111211"></a><a name="1.21.1"/>1.21.1

* Çapraz bölüm sıralama ölçütü sorguları köşe durumlarda sabit KeyNotFoundException için.
* Burada Item özniteliği LINQ sorguları için select yan tümcesinde değil kabul hata düzeltildi.

### <a name="a-name12021202"></a><a name="1.20.2"/>1.20.2

* Aralıklı olarak sonuçlanan belirli yarış koşulları altında isabet hata düzeltildi "Microsoft.Azure.Documents.NotFoundException: Oturum tutarlılık düzeyi kullanılırken okuma oturumu için giriş Oturum belirteci kullanılabilir değil"hata.

### <a name="a-name12011201"></a><a name="1.20.1"/>1.20.1

* Gerilemesi düzeltildi burada FeedOptions.MaxItemCount = -1 bir System.ArithmeticException oluşturdu: sayfa boyutu negatif ise.
* Yeni bir ToString() işlevini QueryMetrics için eklendi.
* Bölüm istatistikleri okuma koleksiyonlar üzerinde açık.
* ChangeFeedOptions için PartitionKey özelliği eklendi.
* Küçük hata düzeltmeleri.

### <a name="a-name11911191"></a><a name="1.19.1"/>1.19.1

* Belgeler için benzersiz dizinler hakkında DocumentCollection UniqueKeyPolicy özelliğini kullanarak belirtme olanağı ekler.
* İçinde özel JsonSerializer ayarları bazı sorguları ve saklı yordam yürütme için kabul değil, bir hata düzeltildi.

### <a name="a-name11901190"></a><a name="1.19.0"/>1.19.0

* Azure Cosmos DB API Başvurusu'ndaki Azure documentdb'den bir değişiklik, belgeler, meta veri bilgilerini derlemelerde ve NuGet paketini marka. 
* Tanılama bilgileri ve gecikme süresi ile doğrudan bağlantı modunu gönderilen isteklerin yanıtından kullanıma sunar. Özellik adlarını, ResourceResponse sınıfı üzerinde RequestDiagnosticsString ve RequestLatency bulunur.
* Bu SDK sürüm Merkezi'nden Azure Cosmos DB Emulator kullanılabilir en son sürümünü gerektirir. https://aka.ms/cosmosdb-emulator. 

### <a name="a-name11811181"></a><a name="1.18.1"/>1.18.1 

* İç değişiklik Microsoft arkadaş derlemeleri için.

### <a name="a-name11801180"></a><a name="1.18.0"/>1.18.0 

* Birçok güvenilirlik düzeltmeleri ve geliştirmeleri eklendi.

### <a name="a-name11701170"></a><a name="1.17.0"/>1.17.0 

* Sorgu Sonuçları belirli bir bölüm anahtar aralığı değeri için kapsam için bir FeedOption olarak PartitionKeyRangeId desteği eklendi. 
* Bundan sonra değişikliklerin bakmaya başlamak için bir ChangeFeedOption olarak StartTime desteği eklendi.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Bir yığın taşması özel durumuna neden olabilir JsonSerializable sınıfında bir sorun düzeltildi.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
*   DocumentClient Oluşturucusu isteğe bağlı bir parametre olarak JsonSerializerSettings tanıtılması nedeniyle uygulama yeniden derlenmesi gereken bir sorun düzeltildi.
* DocumentClient oluşturucusuna geçersiz ConsistencyLevel parametreleri ve varsayılan değerleri için ConnectionPolicy izin vermek için son parametre olarak, gerekli JsonSerializerSettings JsonSerializerSettings parametresinde geçerken işaretlenmiş.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
*   Özel bir JsonSerializerSettings örneği oluşturulurken belirtme desteği eklendi [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name11411141"></a><a name="1.14.1"/>1.14.1
*   X64 etkilenen bir sorun düzeltildi SSE4 yönerge desteği yoktur ve Azure Cosmos DB SQL sorgu çalıştırırken bir SEHException throw makineleri.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*   Yeni bir tutarlılık düzeyi için destek eklendi ConsistentPrefix çağrılır.
*   Tek tek bölümler için sorgu ölçümler için destek eklendi.
*   Sorgular için devamlılık belirteci boyutunu sınırlamak için destek eklendi.
*   Başarısız istekler için ayrıntılı izleme desteği eklendi.
*   SDK'ın bazı performans iyileştirmeleri yapıldı.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* 1\.13.3 işlevsel olarak aynıdır. İç bazı değişiklikler yaptınız.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* 1\.13.2 işlevsel olarak aynıdır. İç bazı değişiklikler yaptınız.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Toplam sorgularında FeedOptions içinde sağlanan PartitionKey değeri yok sayıldı bir sorun düzeltildi.
* Bölümler arası sorgu yürütme Order By Orta uçuş sırasında bölüm yönetimi saydam işlemede bir sorun düzeltildi.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* Bazı zaman uyumsuz API'leri ASP.NET bağlam içinde kullanıldığında kilitlenmeleri neden bir sorun düzeltildi.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Belirli koşullar altında otomatik yük devretme karşı daha dayanıklı SDK yapmak için düzeltmeleri.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Bazen bir WebException neden olan bir sorunu için düzeltme: Uzak ad çözümlenemedi.
* Doğrudan ReadDocumentAsync API'sine yeni aşırı yüklemeler ekleyerek türü belirtilmiş bir belgeyi okumak için destek eklendi.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için LINQ desteği eklendi.
* Olay işleyicisi kullanımından kaynaklanan ConnectionPolicy nesne için bir bellek sızıntısı sorunu düzeltildi.
* ETag kullanıldığında burada görüntülerle UpsertAttachmentAsync çalıştığı olmayan bir sorunu düzeltin.
* Burada görüntülerle sorgu devamlılığı sırası tarafından çapraz bölüm sıralarken dize alanı üzerinde çalıştığı değil bir sorun düzeltildi.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için destek eklendi. Bkz: [toplama Destek](sql-query-aggregates.md).
* Bölümlenmiş koleksiyonlardan 10,100 RU/sn 2500 RU/sn için en düşük aktarım hızını düşürdü.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* Gibi bazı bölümler arası sorgular 32-bit ana işlemde başarısız olduğu sorun düzeltildi.
* Burada görüntülerle oturumu kapsayıcı ağ geçidi modunda başarısız istekler için belirteci ile güncelleştirilmedi bir sorun düzeltildi.
* Bazı durumlarda sorguda projeksiyon UDF çağrılarında burada görüntülerle gönderilememesi sorunu düzeltildi.
* İstemci tarafı performans okuma ve yazma istekleri verimini artırmak için düzeltir.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Burada görüntülerle oturumu kapsayıcı başarısız istekler için belirteci ile güncelleştirilmedi bir sorun düzeltildi.
* Bir 32-bit ana işlem olarak çalışacak şekilde SDK için destek eklendi. Çapraz bölüm sorguları kullanıyorsanız, 64-bit ana işleme daha iyi performans için önerilen unutmayın.
* Çok sayıda bölüm anahtarı değerlerine IN deyimde sorgularıyla ilgili senaryolar için performansı İyileştirildi.
* Çeşitli kaynak kotası istatistikleri ResourceResponse belge koleksiyonu PopulateQuotaInfo isteği seçenek ayarlandığında okuma istekleri için de doldurulur.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* İçinde 1.11.0 Createdocumentcollectionıfnotexistsasync API'si için küçük bir performans düzeltme.
* Performans, yüksek derecede eşzamanlı istek gerektiren senaryolar için SDK'sındaki düzeltin.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Yeni sınıflar ve işlemek için yöntemler için destek [değişiklik akışını](change-feed.md) , bir koleksiyonu içindeki belgeler.
* Bölümler arası sorgu devamlılık ve bölümler arası sorgular için bazı performans geliştirmeleri için destek.
* Ayrıca Createdatabaseasync ve Createdocumentcollectionıfnotexistsasync yöntemleri.
* Sistem işlevleri için LINQ desteği: IsDefined, IsNull ve IsPrimitive.
* Project.json araç projeleri ile Nuget paketini kullanarak uygulamanın bin klasörüne Microsoft.Azure.Documents.ServiceInterop.dll ve DocumentDB.Spatial.Sql.dll derlemelerin otomatik binplacing düzeltildi.
* Hata ayıklama senaryoları yararlı olabilecek istemci tarafı ETW izlemeleri yayma desteği.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Bölümlenmiş koleksiyonlar için eklenen doğrudan bağlantı desteği.
* Sınırlanmış eskime durumu tutarlılık düzeyi için performansı İyileştirildi.
* Eklenen Çokgen ve dizin oluşturma ilkesi şirketin coğrafı uzamsal sorgular için koleksiyon belirtirken LineString veri türleri.
* Koşullar çevrilirken LINQ için destek eklendi StringEnumConverter ve IsoDateTimeConverter UnixDateTimeConverter.
* SDK çeşitli hata düzeltmeleri.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Aşağıdaki NotFoundException kaynaklanan bir sorun düzeltildi: Okuma oturum giriş Oturum belirteci için kullanılabilir değil. Bazı durumlarda coğrafi olarak dağıtılmış bir hesabı okuma-bölgesi için sorgulama bu özel durum oluştu.
* Doğrudan erişim için temel alınan akışa bir yanıt sağlar sınıfını ResourceResponse ResponseStream özelliğinde ortaya çıkar.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Böylece dağıtım (TDD) tabanlı test için örnek karşılık gelen ortak arabirim uygulamak için ResourceResponse, FeedResponse StoredProcedureResponse ve MediaResponse sınıfları değiştirildi.
* Hatalı biçimlendirilmiş bölüm anahtarı üstbilgi verilerin serileştirilmesi için özel bir JsonSerializerSettings nesne kullanırken neden olan bir sorun düzeltildi.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Uzun süren sorgular başarısız olmasına neden olan hata ile bir sorun düzeltildi: Şu anda yetkilendirme belirteci geçerli değil.
* Çapraz bölüm üst/sırası-by sorguları gelen özgün SqlParameterCollection kaldırılan bir sorun düzeltildi.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Bölümlenmiş koleksiyonlar için paralel sorgular için destek eklendi.
* Çapraz bölüm ORDER BY ve üst sorguları bölümlenmiş koleksiyonlar için eklenen desteği.
* Azure Cosmos DB Nuget paketine başvuru ile bir Azure Cosmos DB projesi başvururken kullanılır eksik başvurular DocumentDB.Spatial.Sql.dll ve Microsoft.Azure.Documents.ServiceInterop.dll düzeltildi.
* Kullanıcı tanımlı işlevler üzerinde LINQ kullanırken farklı türde parametreler kullanma olanağı düzeltildi. 
* Burada Upsert çağrıları okuma yazma konumlar yerine konumları yönlendirilmesi genel olarak çoğaltılan hesapları için bir hata düzeltildi.
* Eksik eklenen yöntemleri IDocumentClient arabirimine: 
  * MediaStream ve seçenekleri parametre olarak alan UpsertAttachmentAsync yöntemi
  * Seçenekler, parametre olarak alır CreateAttachmentAsync yöntemi
  * CreateOfferQuery yöntemi querySpec bir parametre olarak alır.
* IDocumentClient arabiriminde sunulan korumasız Genel sınıflar.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Çoklu bölge veritabanı hesapları için destek eklendi.
* Yeniden deneme sırasında daraltılmış istekler için destek eklendi.  Kullanıcı sayısını yeniden denemeler ve en fazla bekleme zamanı ConnectionPolicy.RetryOptions özelliğini yapılandırarak özelleştirebilirsiniz.
* Tüm DocumentClient özellik ve yöntem imzaları tanımlayan yeni IDocumentClient arabirimi eklendi.  Bu değişikliğin bir parçası olarak Iqueryable IOrderedQueryable DocumentClient sınıfındaki yöntemleri oluşturup genişletme yöntemleri de değiştirildi.
* Belirli Azure Cosmos DB uç noktası URI'si için ServicePoint.ConnectionLimit ayarlamak için ek yapılandırma seçeneği'ni kullanın.  ConnectionPolicy.MaxConnectionLimit 50 olarak ayarlandığını varsayılan değeri değiştirmek için kullanın.
* Kullanım dışı IPartitionResolver ve uygulanması.  IPartitionResolver desteği artık kullanımdan kalkmıştır. Daha yüksek depolama ve aktarım hızı için bölümlenmiş koleksiyonlar kullanmanız önerilir.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* RequestOptions parametresi olarak alan ExecuteStoredProcedureAsync yöntemi aşırı URI'ye dayalı eklendi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Belgeler için eklenen zaman canlı (TTL) desteği.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* .NET SDK'sı Nuget paketlenmesi, bir Azure bulut hizmeti çözümün bir parçası olarak paketlemek için düzeltildi.

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* Uygulanan [bölümlenmiş koleksiyonları](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri](performance-levels.md). 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Düzeltildi]**  Sorgulama Azure Cosmos DB uç noktası oluşturur: 'System.Net.Http.HttpRequestException: Bir akışa içeriği kopyalanırken hata oluştu '.

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Genişletilmiş LINQ, disk belleği, koşullu ifade yeni işleçleri dahil olmak üzere destek ve karşılaştırma aralığı.
  * LINQ seçin üst davranışı etkinleştirmek için işleci Al
  * CompareTo işleci dize aralığı karşılaştırmaları etkinleştirmek için
  * Koşullu (?) ve birleşim işleçlerini (?)
* **[Düzeltildi]**  Modeli projeksiyon ile birleştirilirken üretiliyor nerede bileşeninde bir LINQ Sorgu. [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Düzeltildi]**  LINQ sağlayıcısı seçin son deyim değilse, projeksiyon kabul ve SELECT üretilen * yanlış.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Uygulanan Upsert, eklenen UpsertXXXAsync yöntemleri
* Tüm istekler için performans geliştirmeleri
* LINQ sağlayıcı desteği koşullu, coalesce ve dizeler için CompareTo Yöntemi
* **[Düzeltildi]**  LINQ sağlayıcısı--> IEnumerable ve dizi gibi aynı SQL oluşturulacak liste yöntemi uygulama içerir
* **[Düzeltildi]**  BackoffRetryUtility kullandığı aynı HttpRequestMessage yeniden denemedeki yeni bir tane oluşturmak yerine
* **[Eski]**  UriFactory.CreateCollection--> UriFactory.CreateDocumentCollection kullanmak yerine artık

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Düzeltildi]**  Yerelleştirme, nl-NL, vb. gibi kültür bilgisi olmayan tr kullanırken sorunları. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Kimlik tabanlı yönlendirme eklendi
  * Kimlik tabanlı kaynak bağlantıları oluşturma ile yardımcı olmak için yeni UriFactory Yardımcısı
  * Yeni aşırı yüklemeler URI almak için DocumentClient üzerinde
* IsValid() ve LINQ için Jeo-uzamsal IsValidDetailed() eklendi
* Gelişmiş LINQ sağlayıcı desteği:
  * **Matematik** -Abs, Acos, Asin, Atan, Exp, Floor, günlük, Log10, Pow, hepsini, oturum, Sin, Sqrt, Tan Cos Ceiling Kes
  * **Dize** -EndsWith, IndexOf, sayısı, ToLower, TrimStart Concat, içerir, Değiştir, ters TrimEnd, StartsWith, SubString, ToUpper
  * **Dizi** -Concat, içerir, sayısı
  * **IN** işleci

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Dizin oluşturma ilkeleri değiştirmek için destek eklendi.
  * DocumentClient yeni ReplaceDocumentCollectionAsync yöntemi
  * Yeni IndexTransformationProgress özelliğinde ResourceResponse<T> dizin İlkesi değişikliklerinin yüzde ilerlemesini izlemek için
  * DocumentCollection.IndexingPolicy değişebilir
* Uzamsal dizin oluşturma ve sorgu için destek eklendi.
  * Serileştirme/uzamsal türler seri durumdan çıkarmak için yeni Microsoft.Azure.Documents.Spatial ad alanı, nokta ve Çokgen gibi
  * Cosmos DB'de depolanan GeoJSON veri dizinini oluşturmak için yeni SpatialIndex sınıfı
* **[Düzeltildi]**  Bir LINQ ifadesini oluşturulan hatalı SQL sorgusu [#38](https://github.com/Azure/azure-documentdb-net/issues/38).

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Bir bağımlılık Newtonsoft.Json v5.0.7 üzerinde eklendi.
* Order By desteklemek için değişiklikler:
  
  * LINQ sağlayıcı desteği OrderBy() veya OrderByDescending()
  * Order By desteklemek için IndexingPolicy 
    
    **Olası değişiklik** 
    
    Özel bir dizin oluşturma ilkesini bu hükümleri koleksiyonlarla mevcut kodunuz varsa, mevcut kodunuzu yeni IndexingPolicy sınıfı destekleyecek şekilde güncelleştirilmesi gerekir. Ardından, hiçbir özel dizin oluşturma ilkesi varsa, bu değişiklik, etkilemez.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Yeni HashPartitionResolver ve RangePartitionResolver sınıfları ve IPartitionResolver kullanarak verileri bölümleme için destek eklendi.
* DataContract serileştirme eklendi.
* Eklenen GUID LINQ sağlayıcısı destekler.
* Eklenen UDF LINQ destekler.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK'SI

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş hafifletmek için bir SDK'yı devre dışı bırakmadan önce.

Geçerli SDK'sı yalnızca eklenen yeni özellikler ve işlevsellik ve en iyi duruma getirme, bu nedenle, her zaman en son SDK sürümüne erken mümkün olduğunca yükseltmeniz önerilir. 

Devre dışı bırakılan bir SDK'sı kullanarak Azure Cosmos DB yapılan tüm isteklere hizmet tarafından reddedilir.

<br/>

| Version | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
| [2.5.1](#2.5.1) |02 Temmuz 2019 |--- |
| [2.4.1](#2.4.1) |20 Haziran 2019 |--- |
| [2.4.0](#2.4.0) |05 Mayıs 2019 |--- |
| [2.3.0](#2.3.0) |04 Nisan 2019 |--- |
| [2.2.3](#2.2.3) |11 Şubat 2019 |--- |
| [2.2.2](#2.2.2) |06 Şubat 2019 |--- |
| [2.2.1](#2.2.1) |Aralık 24 Mayıs 2018 |--- |
| [2.2.0](#2.2.0) |07 aralık 2018'e |--- |
| [2.1.3](#2.1.3) |15 Ekim 2018 |--- |
| [2.1.2'yi](#2.1.2) |04 Ekim 2018 |--- |
| [2.1.1](#2.1.1) |27 Eylül 2018'den |--- |
| [2.1.0](#2.1.0) |21 Eylül 2018 |--- |
| [2.0.0](#2.0.0) |07 Eylül 2018'den |--- |
| [1.22.0](#1.22.0) |19 Nisan 2018 |--- |
| [1.21.1](#1.20.1) |09 Mart 2018 |--- |
| [1.20.2](#1.20.1) |21 Şubat 2018 |--- |
| [1.20.1](#1.20.1) |05 Şubat 2018 |--- |
| [1.19.1](#1.19.1) |16 Kasım 2017 |--- |
| [1.19.0](#1.19.0) |10 Kasım 2017 |--- |
| [1.18.1](#1.18.1) |07 Kasım 2017 |--- |
| [1.18.0](#1.18.0) |17 Ekim 2017 |--- |
| [1.17.0](#1.17.0) |10 Ağustos 2017 |--- |
| [1.16.1](#1.16.1) |07 Ağustos 2017 |--- |
| [1.16.0](#1.16.0) |02 Ağustos 2017 |--- |
| [1.15.0](#1.15.0) |30 Haziran 2017 |--- |
| [1.14.1](#1.14.1) |23 Mayıs 2017 |--- |
| [1.14.0](#1.14.0) |10 Mayıs 2017 |--- |
| [1.13.4](#1.13.4) |09 Mayıs 2017 |--- |
| [1.13.3](#1.13.3) |06 Mayıs 2017 |--- |
| [1.13.2](#1.13.2) |19 Nisan 2017 |--- |
| [1.13.1](#1.13.1) |29 Mart 2017 |--- |
| [1.13.0](#1.13.0) |24 Mart 2017 |--- |
| [1.12.2](#1.12.2) |20 Mart 2017 |--- |
| [1.12.1](#1.12.1) |14 Mart 2017 |--- |
| [1.12.0](#1.12.0) |15 Şubat 2017 |--- |
| [1.11.4](#1.11.4) |06 Şubat 2017 |--- |
| [1.11.3](#1.11.3) |26 Ocak 2017 |--- |
| [1.11.1](#1.11.1) |21 aralık 2016 |--- |
| [1.11.0](#1.11.0) |08 Aralık 2016 |--- |
| [1.10.0](#1.10.0) |27 Eylül 2016 |--- |
| [1.9.5](#1.9.5) |01 Eylül 2016 |--- |
| [1.9.4](#1.9.4) |24 Ağustos 2016 |--- |
| [1.9.3](#1.9.3) |15 Ağustos 2016 |--- |
| [1.9.2](#1.9.2) |23 Temmuz 2016 |--- |
| [1.8.0](#1.8.0) |14 Haziran 2016 |--- |
| [1.7.1](#1.7.1) |06 Mayıs 2016 |--- |
| [1.7.0](#1.7.0) |26 Nisan 2016 |--- |
| [1.6.3](#1.6.3) |08 Nisan 2016 |--- |
| [1.6.2](#1.6.2) |29 Mart 2016 |--- |
| [1.5.3](#1.5.3) |19 Şubat 2016 |--- |
| [1.5.2](#1.5.2) |14 Aralık 2015 |--- |
| [1.5.1](#1.5.1) |November 23, 2015 |--- |
| [1.5.0](#1.5.0) |05 Ekim 2015 |--- |
| [1.4.1](#1.4.1) |25 Ağustos 2015 |--- |
| [1.4.0](#1.4.0) |13 Ağustos 2015 |--- |
| [1.3.0](#1.3.0) |05 Ağustos 2015 |--- |
| [1.2.0](#1.2.0) |06 Temmuz 2015 |--- |
| [1.1.0](#1.1.0) |30 Nisan 2015 |--- |
| [1.0.0](#1.0.0) |08 Nisan 2015 |--- |


## <a name="faq"></a>SSS
[!INCLUDE [cosmos-db-sdk-faq](../../includes/cosmos-db-sdk-faq.md)]

## <a name="see-also"></a>Ayrıca bkz.
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmeti sayfası. 

