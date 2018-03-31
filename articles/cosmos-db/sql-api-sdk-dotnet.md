---
title: 'Azure Cosmos DB: SQL .NET API, SDK & kaynakları | Microsoft Docs'
description: SQL .NET API ve yayın tarih, sona erme tarihlerini ve her Azure Cosmos DB .NET SDK'sı sürüm arasında yapılan değişiklikler dahil olmak üzere SDK'sı hakkında bilgi edinin.
services: cosmos-db
documentationcenter: .net
author: rnagpal
manager: jhubbard
editor: cgronlun
ms.assetid: 8e239217-9085-49f5-b0a7-58d6e6b61949
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/09/2018
ms.author: rnagpal
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 662a1d1d0f13b64cc87ab6eb0eee6af94cd97c54
ms.sourcegitcommit: 34e0b4a7427f9d2a74164a18c3063c8be967b194
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/30/2018
---
# <a name="azure-cosmos-db-net-sdk-for-sql-api-download-and-release-notes"></a>SQL API için Azure Cosmos DB .NET SDK: indirme ve sürüm notları
> [!div class="op_single_selector"]
> * [.NET](sql-api-sdk-dotnet.md)
> * [.NET değişiklik besleme](sql-api-sdk-dotnet-changefeed.md)
> * [.NET Core](sql-api-sdk-dotnet-core.md)
> * [Node.js](sql-api-sdk-node.md)
> * [Async Java](sql-api-sdk-async-java.md)
> * [Java](sql-api-sdk-java.md)
> * [Python](sql-api-sdk-python.md)
> * [REST](https://docs.microsoft.com/rest/api/cosmos-db/)
> * [REST Kaynak Sağlayıcısı](https://docs.microsoft.com/rest/api/cosmos-db-resource-provider/)
> * [SQL](https://msdn.microsoft.com/library/azure/dn782250.aspx)
> 
> 

<table>

<tr><td>**SDK yükleme**</td><td>[NuGet](https://www.nuget.org/packages/Microsoft.Azure.DocumentDB/)</td></tr>

<tr><td>**API belgeleri**</td><td>[.NET API başvuru belgeleri](/dotnet/api/overview/azure/cosmosdb?view=azure-dotnet)</td></tr>

<tr><td>**Örnekler**</td><td>[.NET kodu örnekleri](sql-api-dotnet-samples.md)</td></tr>

<tr><td>**Kullanmaya başlama**</td><td>[Azure Cosmos DB .NET SDK ile çalışmaya başlama](sql-api-get-started.md)</td></tr>

<tr><td>**Web uygulaması Öğreticisi**</td><td>[Azure Cosmos DB ile Web uygulaması geliştirme](sql-api-dotnet-application.md)</td></tr>

<tr><td>**Geçerli desteklenen çerçevelerden**</td><td>[Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653)</td></tr>
</table></br>

## <a name="release-notes"></a>Sürüm notları
### <a name="a-name12111211"></a><a name="1.21.1"/>1.21.1

* Bölüm düzeni köşe durumlarda sorgular tarafından çapraz sabit KeyNotFoundException için.
* Sabit hata burada JsonPropery özniteliği LINQ sorguları için select yan tümcesinde değil dikkate alınır.

### <a name="a-name12021202"></a><a name="1.20.2"/>1.20.2

* Aralıklı olarak sonuçları için belirli yarış koşullarda isabet sabit hata "Microsoft.Azure.Documents.NotFoundException: Okuma oturum giriş Oturum belirteci için kullanılabilir değil" oturum tutarlılığı düzeyini kullanırken hataları.

### <a name="a-name12011201"></a><a name="1.20.1"/>1.20.1

* Regresyon sabit nerede FeedOptions.MaxItemCount = -1 bir System.ArithmeticException oluşturdu: sayfa boyutu negatiftir.
* Yeni bir ToString() işlevini QueryMetrics için eklendi.
* Bölüm istatistikleri okuma koleksiyonlar üzerinde açık.
* Eklenen PartitionKey özelliğine ChangeFeedOptions.
* İkincil hata düzeltmeleri.

### <a name="a-name11911191"></a><a name="1.19.1"/>1.19.1

* Üzerinde DocumentCollection UniqueKeyPolicy özelliğini kullanarak belgeler için benzersiz dizin belirtme olanağı ekler.
* İçinde özel JsonSerializer ayarları bazı sorguları ve saklı yordam yürütme için dikkate alınır değil hatanın düzeltildiğini.

### <a name="a-name11901190"></a><a name="1.19.0"/>1.19.0

* API Başvurusu Azure Cosmos DB Azure DocumentDB değişikliği marka belgeleri, meta veri bilgileri derlemelerde ve NuGet paketi. 
* Tanılama bilgileri ve gecikme süresi ile doğrudan bağlantı modunu gönderilen istekleri yanıt gelen kullanıma sunar. Özellik adlarının RequestDiagnosticsString ve RequestLatency ResourceResponse sınıfı üzerinde bulunur.
* Bu SDK sürümü Merkezi'nden Azure Cosmos DB öykünücüsü kullanılabilir en son sürümünü gerektirir https://aka.ms/cosmosdb-emulator. 

### <a name="a-name11811181"></a><a name="1.18.1"/>1.18.1 

* Microsoft arkadaş derlemeler için iç değişiklikleri.

### <a name="a-name11801180"></a><a name="1.18.0"/>1.18.0 

* Birçok güvenilirlik düzeltmeleri ve geliştirmeleri eklendi.

### <a name="a-name11701170"></a><a name="1.17.0"/>1.17.0 

* Sorgu Sonuçları belirli bir bölüm anahtarı aralığının değerine kapsamı için bir FeedOption olarak PartitionKeyRangeId desteği eklendi. 
* Bundan sonra değişikliklerin aramaya başlamak için bir ChangeFeedOption olarak StartTime desteği eklendi.

### <a name="a-name11611161"></a><a name="1.16.1"/>1.16.1
* Bir yığın taşması özel durumu neden olabilir JsonSerializable sınıfında bir sorun düzeltilmiştir.

### <a name="a-name11601160"></a><a name="1.16.0"/>1.16.0
*   Bir sorun nedeniyle JsonSerializerSettings giriş uygulamanın DocumentClient Oluşturucusu isteğe bağlı bir parametre olarak yeniden derlenmesi gereken sabit.
* DocumentClient Oluşturucusu artık kullanılmayan ConsistencyLevel parametreleri ve ConnectionPolicy varsayılan değerler için izin vermek için son parametre olarak bu gerekli JsonSerializerSettings JsonSerializerSettings parametresinde geçirilirken olarak işaretlenmiş.

### <a name="a-name11501150"></a><a name="1.15.0"/>1.15.0
*   Örnek oluşturma sırasında özel JsonSerializerSettings belirtmek için destek eklenmiştir [DocumentClient](/dotnet/api/microsoft.azure.documents.client.documentclient?view=azure-dotnet).

### <a name="a-name11411141"></a><a name="1.14.1"/>1.14.1
*   X64 etkilenen bir sorun sabit olmayan SSE4 yönerge destekleyen ve bir SEHException Azure Cosmos DB SQL sorgu çalıştırırken throw makineler.

### <a name="a-name11401140"></a><a name="1.14.0"/>1.14.0
*   Yeni bir tutarlılık düzeyi için destek eklendi ConsistentPrefix çağrılır.
*   Tek tek bölümler için sorgu ölçümleri desteği eklendi.
*   Sorgular için devamlılık belirteci boyutunu sınırlamak için destek eklendi.
*   Başarısız istekler için ayrıntılı izleme desteği eklendi.
*   SDK'ın bazı performans geliştirmeleri yapıldı.

### <a name="a-name11341134"></a><a name="1.13.4"/>1.13.4
* 1.13.3 işlevsel olarak aynıdır. İç bazı değişiklikler yaptınız.

### <a name="a-name11331133"></a><a name="1.13.3"/>1.13.3
* 1.13.2 işlevsel olarak aynıdır. İç bazı değişiklikler yaptınız.

### <a name="a-name11321132"></a><a name="1.13.2"/>1.13.2
* Toplama sorguları için FeedOptions içinde sağlanan PartitionKey değeri göz ardı bir sorun düzeltilmiştir.
* Bölüm yönetim saydam işleme Orta uçuş çapraz bölüm Order By sorgu yürütme sırasında bir sorun sabit.

### <a name="a-name11311131"></a><a name="1.13.1"/>1.13.1
* Zaman uyumsuz ASP.NET bağlam içinde kullanıldığında API'leri bazılarını kilitlenmeleri neden bir sorun düzeltilmiştir.

### <a name="a-name11301130"></a><a name="1.13.0"/>1.13.0
* Otomatik Yük devretme belirli koşullar altında daha esnektir SDK yapmak giderir.

### <a name="a-name11221122"></a><a name="1.12.2"/>1.12.2
* Bazen WebException neden olan bir sorunu düzeltin: uzak ad çözümlenemedi.
* Doğrudan ReadDocumentAsync API'sine yeni aşırı ekleyerek yazılı belgeyi okumak için destek eklendi.

### <a name="a-name11211121"></a><a name="1.12.1"/>1.12.1
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) için LINQ destek eklenmiştir.
* Olay işleyicisi kullanımına neden ConnectionPolicy nesnesi için bellek sızıntısı sorunu düzeltin.
* ETag kullanıldığında; burada görüntülerle UpsertAttachmentAsync çalışır durumda olmayan bir sorunu düzeltin.
* Burada görüntülerle sorgu devamlılığı sırası tarafından çapraz bölüm sıralarken dize alanı üzerinde çalıştığı olmayan bir sorunu düzeltin.

### <a name="a-name11201120"></a><a name="1.12.0"/>1.12.0
* Toplama sorguları (sayısı, MIN, MAX, toplam ve ortalama) desteği eklendi. Bkz: [toplama Destek](sql-api-sql-query.md#Aggregates).
* En düşük işleme 2500 RU/s 10,100 RU/s bölümlenmiş koleksiyonlar üzerinde düşürdü.

### <a name="a-name11141114"></a><a name="1.11.4"/>1.11.4
* Burada görüntülerle bazı çapraz bölüm sorguları 32-bit ana işleminde başarısız bir sorun için düzeltme.
* Burada görüntülerle oturum kapsayıcı ağ geçidi modunda başarısız istekler için belirteci ile güncelleştirilmedi bir sorun için düzeltme.
* Bazı durumlarda bir sorgu projeksiyon UDF çağrılarında; burada görüntülerle başarısız sorunu düzeltin.
* İstemci tarafı performans isteklerin okuma ve yazma verimini artırmak için giderir.

### <a name="a-name11131113"></a><a name="1.11.3"/>1.11.3
* Burada görüntülerle oturum kapsayıcı başarısız istekler için belirteci ile güncelleştirilmedi bir sorun için düzeltme.
* İş bir 32 bit ana bilgisayar işlemi için SDK desteği eklendi. Not çapraz bölüm sorguları kullanıyorsanız, 64-bit ana bilgisayar işleme Gelişmiş performans için önerilir.
* Çok sayıda bölüm anahtarı değerleri IN deyimde sorgularıyla ilgili senaryoları için geliştirilmiş performans.
* Belge koleksiyonunu PopulateQuotaInfo isteği seçenek ayarlandığında okuma istekleri ResourceResponse içinde çeşitli kaynak kotası istatistikleri doldurulur.

### <a name="a-name11111111"></a><a name="1.11.1"/>1.11.1
* 1.11.0 içinde sunulan CreateDocumentCollectionIfNotExistsAsync API için küçük performans düzeltme.
* Performans düzeltme yüksek derecede eşzamanlı istek gerektiren senaryolar için SDK.

### <a name="a-name11101110"></a><a name="1.11.0"/>1.11.0
* Yeni sınıflar ve yöntemler işlemek için destek [akış değiştirmek](change-feed.md) bir koleksiyon içinde belgelerin.
* Çapraz bölüm sorgusu devamlılık ve çapraz bölüm sorgular için bazı performans geliştirmeleri için destek.
* Ayrıca CreateDatabaseIfNotExistsAsync ve CreateDocumentCollectionIfNotExistsAsync yöntemleri.
* LINQ sistem işlevleri için destek: IsDefined, IsNull ve IsPrimitive.
* Uygulamanın bin klasörüne Microsoft.Azure.Documents.ServiceInterop.dll ve DocumentDB.Spatial.Sql.dll derlemelerin otomatik binplacing için Nuget paketi project.json araç projeleri ile kullanırken düzeltin.
* Senaryoları hata ayıklamaya yardımcı olabilir istemci tarafı ETW izlerini yayma desteği.

### <a name="a-name11001100"></a><a name="1.10.0"/>1.10.0
* Bölümlenmiş koleksiyonlar için eklenen doğrudan bağlantı desteği.
* Sınırlanmış eskime durumu tutarlılığı düzeyi için performansı'yi tıklatın.
* Eklenen Çokgen ve LineString İlkesi coğrafı uzamsal sorguları için dizin oluşturma koleksiyonunu belirten sırasında veri türleri.
* Koşulları çevrilirken LINQ desteği eklendi StringEnumConverter, IsoDateTimeConverter ve UnixDateTimeConverter.
* Çeşitli SDK hata düzeltmeleri.

### <a name="a-name195195"></a><a name="1.9.5"/>1.9.5
* Aşağıdaki NotFoundException neden olan sorunu sabit: Okuma oturum giriş Oturum belirteci için kullanılabilir değil. Coğrafi olarak dağıtılan bir hesap okuma-bölge için sorgulama bazı durumlarda bu özel durum oluştu.
* Temel alınan akışa yanıt doğrudan erişim sağlayan ResourceResponse sınıfı ResponseStream özelliğinde açık.

### <a name="a-name194194"></a><a name="1.9.4"/>1.9.4
* Böylece dağıtım (TDD) güdümlü test için mocked karşılık gelen ortak arabirimi uygulamak için ResourceResponse, FeedResponse, StoredProcedureResponse ve MediaResponse sınıfları değiştirdi.
* Özel bir JsonSerializerSettings nesnesi için verilerin serileştirilmesi kullanırken hatalı biçimlendirilmiş bölüm anahtar üstbilgi neden bir sorun düzeltilmiştir.

### <a name="a-name193193"></a><a name="1.9.3"/>1.9.3
* Hata ile uzun süre çalışan sorguların başarısız olmasına neden olan sorunu sabit: Yetkilendirme belirteci geçerli değil şu anda.
* Gelen özgün SqlParameterCollection bölüm üst/order-by sorguları çapraz kaldırılmış bir sorun düzeltilmiştir.

### <a name="a-name192192"></a><a name="1.9.2"/>1.9.2
* Bölümlenmiş koleksiyonlar için paralel sorgular için destek eklendi.
* Bölümlenmiş koleksiyonlar için bölüm sıralama ve üst sorguları çapraz eklenen desteği.
* Bir Azure Cosmos DB proje Azure Cosmos DB Nuget paketine başvuru ile başvururken gerekli eksik başvuruları DocumentDB.Spatial.Sql.dll ve Microsoft.Azure.Documents.ServiceInterop.dll sabit.
* Farklı türde parametreler kullanıcı tanımlı işlevler LINQ kullanırken özelliği sabit. 
* Genel olarak çoğaltılmış hesapları için bir hata burada Upsert çağrıları yazma konumları yerine konumları okumak için yönlendirilmeye sabit.
* Eksik eklenen yöntemleri IDocumentClient arabirimi için: 
  * MediaStream ve seçenekleri parametre olarak alır UpsertAttachmentAsync yöntemi
  * Parametre olarak seçeneklerini alır CreateAttachmentAsync yöntemi
  * Bir parametre olarak querySpec geçen CreateOfferQuery yöntemi.
* IDocumentClient arabiriminde gösterilen korumasız ortak sınıflar.

### <a name="a-name180180"></a><a name="1.8.0"/>1.8.0
* Bölgeli veritabanı hesaplarını desteği eklendi.
* Yeniden deneme daraltılmış isteklerinde desteği eklendi.  Kullanıcı yeniden deneme ve en fazla bekleme süresi sayısı ConnectionPolicy.RetryOptions özelliğini yapılandırarak özelleştirebilirsiniz.
* Tüm DocumenClient özellikleri ve yöntemleri imzalarını tanımlayan yeni bir IDocumentClient arabirimi eklendi.  Bu değişikliğin bir parçası olarak, aynı zamanda Iqueryable IOrderedQueryable DocumentClient sınıfı yöntemleri için oluşturup genişletme yöntemleri değiştirildi.
* Verilen Azure Cosmos DB uç noktası URI ServicePoint.ConnectionLimit ayarlamak için ek yapılandırma seçeneği.  50 olarak ayarlandığını varsayılan değeri değiştirmek için ConnectionPolicy.MaxConnectionLimit kullanın.
* Kullanım dışı IPartitionResolver ve kendi uygulama.  IPartitionResolver desteği artık kullanılmıyor. Daha yüksek depolama ve işleme için bölümlenmiş koleksiyonlar kullanmanız önerilir.

### <a name="a-name171171"></a><a name="1.7.1"/>1.7.1
* RequestOptions bir parametre olarak alır ExecuteStoredProcedureAsync yöntemi aşırı URI'ye dayalı eklendi.

### <a name="a-name170170"></a><a name="1.7.0"/>1.7.0
* Belgeler için ek süre dinamik (TTL) desteği.

### <a name="a-name163163"></a><a name="1.6.3"/>1.6.3
* Bir hata, bir Azure bulut hizmeti çözümü parçası olarak paketlemek için .NET SDK'sı Nuget paketleme, sabit.

### <a name="a-name162162"></a><a name="1.6.2"/>1.6.2
* Uygulanan [bölümlenmiş koleksiyonlar](partition-data.md) ve [kullanıcı tanımlı performans düzeyleri](performance-levels.md). 

### <a name="a-name153153"></a><a name="1.5.3"/>1.5.3
* **[Sabit]**  Sorgulama Azure Cosmos DB uç noktası oluşturur: ' System.Net.Http.HttpRequestException: içeriği bir akışa kopyalarken hata oluştu '.

### <a name="a-name152152"></a><a name="1.5.2"/>1.5.2
* Genişletilmiş LINQ sayfalama, koşullu ifadeleri için yeni işleçleri dahil olmak üzere destek ve karşılaştırma aralığı.
  * Take LINQ seçin üst davranışını etkinleştirmek için işleci
  * Dize aralığı karşılaştırmaları etkinleştirmek için CompareTo işleci
  * Koşullu (?) ve işleçler (?) birleşim
* **[Sabit]**  Modeli projeksiyon ile birleştirilirken ArgumentOutOfRangeException Where bileşeninde LINQ sorgusu. [#81](https://github.com/Azure/azure-documentdb-dotnet/issues/81)

### <a name="a-name151151"></a><a name="1.5.1"/>1.5.1
* **[Sabit]**  Seçin son deyim değilse LINQ sağlayıcısı projeksiyon olduğu varsayılır ve SELECT üretilen * yanlış.  [#58](https://github.com/Azure/azure-documentdb-dotnet/issues/58)

### <a name="a-name150150"></a><a name="1.5.0"/>1.5.0
* Uygulanan Upsert, eklenen UpsertXXXAsync yöntemleri
* Tüm istekleri için performans iyileştirmeleri
* Koşullu, LINQ sağlayıcı desteği birleşim ve dizeleri CompareTo yöntemleri
* **[Sabit]**  LINQ sağlayıcısı--> IEnumerable ve dizi gibi aynı SQL oluşturmak için listedeki yöntemi uygulama içerir
* **[Sabit]**  BackoffRetryUtility kullandığı aynı HttpRequestMessage Yeniden Dene'yi üzerinde yeni bir tane oluşturmak yerine
* **[Geçersiz]**  UriFactory.CreateCollection--> şimdi UriFactory.CreateDocumentCollection kullanmanız gerekir

### <a name="a-name141141"></a><a name="1.4.1"/>1.4.1
* **[Sabit]**  Yerelleştirme sorunları olmayan tr kültür bilgisi nl-NL, vb. gibi kullanırken. 

### <a name="a-name140140"></a><a name="1.4.0"/>1.4.0
* Kimliği tabanlı yönlendirme eklendi
  * Kaynak Kimliği tabanlı bağlantıları oluşturma ile yardımcı olmak için yeni UriFactory Yardımcısı
  * URI'de almak için DocumentClient üzerinde yeni aşırı yüklemeleri
* IsValid() ve Jeo-uzamsal LINQ IsValidDetailed() eklendi
* LINQ sağlayıcı desteği geliştirilmiştir:
  * **Matematik** -Abs Acos, Asin, Ceiling Exp, Floor, günlük, Log10, Pow, hepsini, oturum, Sinüs, Sqrt, Tan Cos Atan kesme
  * **Dize** -EndsWith, IndexOf, Count, ToLower, TrimStart Concat, içerir, Değiştir, tersine, TrimEnd, StartsWith, SubString, ToUpper
  * **Dizi** -Concat, içerir, sayısı
  * **IN** işleci

### <a name="a-name130130"></a><a name="1.3.0"/>1.3.0
* Dizin oluşturma ilkeleri değiştirmek için destek eklendi.
  * DocumentClient yeni ReplaceDocumentCollectionAsync yöntemi
  * ResourceResponse yeni IndexTransformationProgress özelliğinde<T> dizin ilke değişikliklerini yüzde ilerlemesini izlemek için
  * DocumentCollection.IndexingPolicy artık değişebilir
* Uzamsal dizin oluşturma ve sorgu için destek eklendi.
  * Seri hale getirme/uzamsal türler seri durumdan çıkarmak için yeni Microsoft.Azure.Documents.Spatial ad alanı gibi noktası ve Çokgen
  * Cosmos DB içinde depolanan GeoJSON verileri dizin oluşturma için yeni SpatialIndex sınıfı
* **[Sabit]**  Yanlış SQL sorgu LINQ ifadesinden [#38](https://github.com/Azure/azure-documentdb-net/issues/38).

### <a name="a-name120120"></a><a name="1.2.0"/>1.2.0
* Bir bağımlılık Newtonsoft.Json v5.0.7 üzerinde eklendi.
* Order By desteklemek için değişiklikler yaptı:
  
  * LINQ sağlayıcı desteği OrderBy() veya OrderByDescending()
  * Order By desteklemek için IndexingPolicy 
    
    **Olası önemli değişiklik** 
    
    Bu hükümleri koleksiyonları özel bir dizin oluşturma ilkesi ile var olan kodu varsa, mevcut kodunuzu yeni IndexingPolicy sınıfı desteklemek üzere güncelleştirilmesi gerekiyor. Daha sonra özel bir dizin oluşturma ilkesi varsa, bu değişiklik, etkilemez.

### <a name="a-name110110"></a><a name="1.1.0"/>1.1.0
* Yeni HashPartitionResolver ve RangePartitionResolver sınıfları ve IPartitionResolver kullanarak veri bölümlendirme için destek eklendi.
* Eklenen DataContract seri hale getirme.
* Eklenen GUID LINQ sağlayıcısında destekler.
* Eklenen UDF LINQ destekler.

### <a name="a-name100100"></a><a name="1.0.0"/>1.0.0
* GA SDK

## <a name="release--retirement-dates"></a>Yayın & sona erme tarihleri
Microsoft'un sağladığı bildirim en az **12 ay** yeni/desteklenen bir sürüme geçiş kesintisiz için bir SDK devre dışı bırakmadan önce.

Yeni özellikler ve işlevsellik ve en iyi duruma getirme geçerli SDK'sı yalnızca eklenir, bu nedenle, her zaman en son SDK sürüme erken mümkün olduğunca yükseltmeniz önerilir. 

Kullanımdan Kaldırılan SDK kullanarak Azure Cosmos DB yapılan tüm isteklere hizmet tarafından reddedilir.

<br/>

| Sürüm | Yayınlanma Tarihi | Sona erme tarihi |
| --- | --- | --- |
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
| [1.14.1](#1.14.1) |23 May 2017 |--- |
| [1.14.0](#1.14.0) |10 Mayıs 2017 |--- |
| [1.13.4](#1.13.4) |09 May 2017 |--- |
| [1.13.3](#1.13.3) |06 May 2017 |--- |
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
| [1.5.1](#1.5.1) |23 Kasım 2015 |--- |
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
Cosmos DB hakkında daha fazla bilgi için bkz: [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hizmet sayfası. 

