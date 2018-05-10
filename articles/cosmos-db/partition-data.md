---
title: Bölümlendirme ve yatay Azure Cosmos DB'de ölçekleme | Microsoft Docs
description: Azure Cosmos DB, bölümleme yapılandırmak ve anahtarları bölümlemek nasıl ve uygulamanız için doğru bölüm anahtarı almak nasıl bölümleme nasıl çalıştığı hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
documentationcenter: ''
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2018
ms.author: rimman
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1976ab5ab0bd0037163b2ad8048fcee10b204ea2
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/07/2018
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Bölüm ve ölçek Azure Cosmos veritabanı

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hızlı ve tahmin edilebilir bir performans elde yardımcı olmak için tasarlanmış bir genel dağıtılmış, birden çok model veritabanı hizmetidir. Bunu büyüdükçe yanı sıra, uygulamanızın sorunsuz bir şekilde ölçeklendirir. Bu makalede Azure Cosmos DB içinde çalıştığı tüm veriler için bölümleme nasıl modeller genel bir bakış sağlar. Ayrıca, uygulamalarınızı etkili bir şekilde ölçeklendirmek için Azure Cosmos DB kapsayıcıları nasıl yapılandırabileceğiniz anlatılmaktadır.

Bölümleme ve bölüm anahtarlarını Bu videoda ele alınmaktadır:

> [!VIDEO https://www.youtube.com/embed/SS6WrQ-HJ30]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB bölümlendirme
Azure Cosmos DB'de depolamak ve herhangi bir ölçeğe bir tek basamaklı milisaniyelik gecikme süresi ile şema daha az veri sorgulayabilirsiniz. Azure Cosmos DB veri depolama adı verilen kapsayıcıları sağlar *koleksiyonları* (belgeler için), *grafikleri*, veya *tabloları*. 

Kapsayıcıları mantıksal kaynaklar ve bir veya daha fazla fiziksel bölümleri veya sunucuları yayılabilir. Bölüm sayısı Azure Cosmos depolama boyutuna göre DB ve bir kapsayıcı veya kapsayıcıları kümesi için sağlanan işlemeye göre belirlenir. 

A *fiziksel* bölümdür ayrılmış SSD yedekli depolama işlem kaynakları (CPU ve bellek) değişken miktarıyla birleştirilmiş sabit bir tutar. Her fiziksel bölüm yüksek kullanılabilirlik için çoğaltılır. Her kümesi kapsayıcıları, bir veya daha fazla fiziksel bölüm paylaşabilir. Fiziksel bölüm yönetimi tam olarak Azure Cosmos DB tarafından yönetilir ve karmaşık kodlar yazmak veya bölüm yönetmek yok. Azure Cosmos DB depolama ve işleme açısından sınırsız kapsayıcılardır. 

A *mantıksal* bölümdür bir bölüm içinde tek bir bölüm anahtar değeriyle ilişkili tüm verileri depolar fiziksel bir bölüm. Birden çok mantıksal bölümler aynı fiziksel bölümünde sonlandırabilirsiniz. Aşağıdaki diyagramda, tek bir kapsayıcı üç mantıksal bölümler vardır. Her mantıksal bölüm için bir bölüm anahtarı, LAX, AMS ve MEL sırasıyla depolamaz. Her LAX, AMS ve MEL mantıksal bölüm 10 GB en büyük mantıksal bölüm sınırı aşan kuramaz. 

![Kaynak bölümlendirme](./media/introduction/azure-cosmos-db-partitioning.png) 

Ne zaman bir kapsayıcı karşılayan [Önkoşullar bölümleme](#prerequisites), bölümleme uygulamanız için tamamen saydam. Azure Cosmos DB hızlı okuma ve yazma, sorguları, işlem mantığı, tutarlılık düzeyleri ve ayrıntılı erişim denetimi yöntemlerini/API'leri tek kapsayıcı kaynağa yoluyla destekler. Hizmet veri fiziksel ve mantıksal bölümleri arasında dağıtma ve doğru bölüm sorgu isteklerin yönlendirmesini işler. 

## <a name="how-does-partitioning-work"></a>Bölümleme nasıl çalışır

Bölümleme nasıl çalışır? Her bir öğe olmalıdır bir *bölüm anahtarı* ve *satır anahtarını*, hangi benzersiz olarak tanımlamak bu. Bölüm anahtarı, verileriniz için bir mantıksal bölüm görevi görür ve fiziksel bölümler veri dağıtılmasında doğal bir sınır ile Azure Cosmos DB sağlar. Tek bir mantıksal bölüm için verileri tek bir fiziksel bölüm içinde bulunmalıdır, ancak fiziksel bölüm yönetim Azure Cosmos DB tarafından yönetilir. 

Kısaca, işte Azure Cosmos DB'de bölümleme nasıl çalışır:

* İle Azure Cosmos DB kapsayıcıları kümesi sağlamak **T** RU/s (saniye başına istek sayısı) işleme.
* Arka planda Azure Cosmos DB sunmak için gereken fiziksel bölümleri sağlar **T** saniye başına istek sayısı. Varsa **T** fiziksel bölüm başına en fazla üretilen daha yüksek **t**, ardından Azure Cosmos DB hükümleri **N T/t =** fiziksel bölümler. Partition(t) başına en fazla üretilen değerini Azure Cosmos DB tarafından yapılandırılmışsa, bu değer toplam sağlanan işleme ve kullanılan donanım yapılandırmasına bağlı olarak atanır. 
* Azure Cosmos DB anahtar alanı bölümünün eşit yatay anahtar karmaları ayırır **N** fiziksel bölümler. Bu nedenle, her fiziksel bölüm konakları **1/N** bölüm anahtarı değerlerini (mantıksal bölümler).
* Fiziksel bir bölüm olduğunda **p** Azure Cosmos DB, depolama sınırına ulaştığında sorunsuz bir şekilde böler **p** iki yeni bölümlere fiziksel, **p1** ve **p2**. Her yeni fiziksel bölüm anahtarları kabaca yarısı karşılık gelen değerleri dağıtır. Bu işlemi bölünmüş uygulamanıza tamamen görünmez durumdadır. Fiziksel bir bölüm, depolama sınırına ulaştığında ve aynı mantıksal bölüm anahtarına ait tüm fiziksel bölümündeki verileri bölme işlemi gerçekleşmez. Bu durum, tek bir mantıksal bölüm anahtarı için tüm veriler aynı fiziksel bölümünde bulunması gerekir çünkü. Bu durumda, farklı bölüm anahtar stratejisi işe.
* Daha yüksek verimlilik sağlamak zaman **t * N**, bir veya daha yüksek verimlilik desteklemek için fiziksel bölümlerinin Azure Cosmos DB böler.

Bölüm anahtarlarını anlamları aşağıdaki tabloda gösterildiği gibi her API semantiği eşleşmesi biraz farklılık gösterir:

| API | Bölüm anahtarı | Satır anahtarı |
| --- | --- | --- |
| SQL | Özel bölüm anahtar yolu | `id` düzeltildi | 
| MongoDB | özel parça anahtarı  | `_id` düzeltildi | 
| Gremlin | Özel bölüm anahtar özelliği | `id` düzeltildi | 
| Tablo | `PartitionKey` düzeltildi | `RowKey` düzeltildi | 

Azure Cosmos DB karma tabanlı bölümleme kullanır. Bir öğe yazdığınızda, Azure Cosmos DB bölüm anahtarı değerini karma hale getirir ve karma hale getirilen sonuç öğesinde depolamak için hangi bölümünü belirlemek için kullanır. Azure Cosmos DB tüm öğeleri aynı fiziksel bölümünde aynı bölüm anahtarına sahip depolar. Bölüm anahtarı seçimi tasarım zamanında yapmak zorunda önemli bir karardır. Çok çeşitli değerleri ve hatta erişim desenlerini sahip bir özellik adı seçin. Fiziksel bir bölüm, depolama sınırına ulaştığında ve bölümünde verileri aynı bölüm anahtarına sahip, Azure Cosmos DB döndürür *"Bölüm anahtarı 10 GB en büyük boyut üst sınırına"* ileti ve bölüm ayrılmaz. İyi bir bölüm anahtarı seçme çok önemli bir karardır.

> [!NOTE]
> Bölüm anahtarı çok sayıda farklı değerleri (örneğin, yüzlerce veya binlerce) ile sağlamak için en iyi bir uygulamadır. İş yükünüzün bu değerleri arasında eşit olarak dağıtmanızı sağlar. İdeal bölüm anahtarı sık sorgularınızı içinde filtre olarak görünür ve çözümünüzü ölçeklenebilir olduğundan emin olmak için yeterli kardinalite olan biridir.
>

Azure Cosmos DB kapsayıcılar olarak oluşturulabilir *sabit* veya *sınırsız* Azure portalında. Sabit boyutlu kapsayıcıların üst sınırı 10 GB ve 10.000 RU/sn aktarım hızıdır. Sınırsız olarak bir kapsayıcı oluşturmak için bir bölüm anahtarı ve en düşük işleme 1.000 RU/s belirtmeniz gerekir. 

Azure Cosmos DB kapsayıcılar ayrıca verimlilik, her kapsayıcı gerekir specificy kapsayıcıların kümesi arasında paylaşmak için yapılandırılabilir bir bölüm anahtarı ve sınırsız büyüyebilir.

Verilerinizi bölümleri arasında nasıl dağıtıldığını denetlemek için iyi bir fikirdir. Portalı'nda veri dağıtım denetlemek için Azure Cosmos DB hesabınıza gidin ve tıklayın **ölçümleri** içinde **izleme** bölümünde ve tıklayın **depolama** görmek için sekmesini nasıl, verileri farklı fiziksel bölümler bölümlenmiş.

![Kaynak bölümlendirme](./media/partition-data/partitionkey-example.png)

Yukarıdaki sol görüntü bozuk bölüm anahtarı sonucunu gösterir ve iyi bir bölüm anahtarı seçildi, yukarıdaki sağ görüntü sonucu gösterir. Sol görüntüde veri bölümleri arasında eşit olarak dağıtılmaz görebilirsiniz. Sağ görüntü benzer şekilde, verilerinizi dağıtan bir bölüm anahtarı seçmek çalışmalarımızı.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Bölümleme için Önkoşullar

Fiziksel bölümler için otomatik Böl **p1** ve **p2** açıklandığı gibi [nasıl bölümleme çalışır](#how-does-partitioning-work), kapsayıcı 1.000 RU/s verimini veya daha fazla ile oluşturulması gerekir (veya paylaşım verimlilik kümesi kapsayıcıları boyunca) ve bir bölüm anahtarı sağlanmalıdır. Bir kapsayıcı (örneğin, bir koleksiyonu, bir grafik veya tablo) Azure portalında oluşturulurken seçin **sınırsız** sınırsız ölçekleme avantajlarından yararlanmak için depolama kapasitesi seçeneği. 

Azure portal veya program aracılığıyla kapsayıcı oluşturuldu ve ilk verimlilik 1.000 RU/s ya da daha fazla, bölüm anahtarı sağlanan oluştuysa, hiçbir değişiklik, kapsayıcı sınırsız ölçekleme, yararlanabilirsiniz. Bu içerir **sabit** kapsayıcıları, ilk kapsayıcı en az 1.000 RU/s ile sn'ye oluşturulur ve bir bölüm anahtarı belirtilen kadar uzun süre.

Kapsayıcıların kümesinin bir parçası üretilen iş paylaşmak üzere yapılandırılmış tüm kapsayıcıları davranılır **sınırsız** kapsayıcıları.

Oluşturduysanız bir **sabit** kapsayıcı hiçbir bölüm anahtarı veya verimlilik değerinden 1.000 RU/s ile kapsayıcı değil-Bu makalede açıklanan POP olur. Bir sınırsız kapsayıcısına (örneğin, bir en az 1.000 RU/s ve bölüm anahtarı) sabit bir kapsayıcı verileri geçirmek için kullanmanız gerekir [veri geçiş aracı](import-data.md) veya [değişiklik akış Kitaplığı](change-feed.md). 

## <a name="partitioning-and-provisioned-throughput"></a>Bölümlendirme ve sağlanan işleme
Azure Cosmos DB tahmin edilebilir performans için tasarlanmıştır. Bir kapsayıcı veya kapsayıcıları kümesi oluşturduğunuzda, iş açısından, yedek  *[istek birimlerine](request-units.md) (RU) saniye başına*. Her istekte CPU, bellek ve g/ç işlemi tarafından tüketilen gibi sistem kaynaklarının miktarını orantılıdır RU gider. Oturum tutarlılığı 1 KB belgeyle okuma 1 tüketir RU. Okuma 1. RU bağımsız olarak depolanan öğeler sayısını veya aynı anda çalıştırılması eşzamanlı istek sayısı. Büyük öğeleri boyutuna bağlı olarak daha yüksek RUs gerektirir. Varlıklarınızı ve uygulamanız için desteklemeniz gereken okuma sayısını boyutunu biliyorsanız, uygulamanızın ihtiyaçları için gereken işleme miktarda sağlayabilirsiniz. 

> [!NOTE]
> Tam olarak bir kapsayıcı veya kapsayıcıları kümesi için sağlanan işleme kullanmasına izin isteklerini tüm farklı bölüm anahtar değerleri arasında eşit olarak dağıtmanızı sağlayan bir bölüm anahtarı seçmeniz gerekir.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Azure Cosmos DB API'leri ile çalışma
Kapsayıcılar oluşturmak ve bunları herhangi bir anda ölçeklendirmek için Azure portalında veya Azure CLI kullanın. Bu bölümde, kapsayıcı oluşturun ve her API kullanarak sağlanan işleme ve bölüm anahtarı belirtin gösterilmektedir.


### <a name="sql-api"></a>SQL API’si
Aşağıdaki örnek, SQL API'yi kullanarak bir kapsayıcı (bir koleksiyon) oluşturulacağını gösterir. 

```csharp
DocumentClient client = new DocumentClient(new Uri(endpoint), authKey);
await client.CreateDatabaseAsync(new Database { Id = "db" });

DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 20000 });
```

Bir öğe (belge) kullanarak okuyabilirsiniz `GET` REST API yönteminde veya kullanarak `ReadDocumentAsync` SDK'lar birinde.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Daha fazla bilgi için bkz: [SQL API'yi kullanarak Azure Cosmos DB içinde bölümleme](sql-api-partition-data.md).

### <a name="mongodb-api"></a>MongoDB API’si
MongoDB API'si ile sık kullanılan aracı, sürücü veya SDK aracılığıyla parçalı bir koleksiyon oluşturabilirsiniz. Bu örnekte, bir koleksiyon oluşturmak için Mongo kabuğunu kullanın.

Mongo Kabuğu'nda:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Sonuçları:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tablo API’si

Tablo API kullanarak bir tablo oluşturmak için kullanmak `CreateIfNotExists` yöntemi. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Sağlanan işleme bağımsız değişken olarak ayarlandığında `CreateIfNotExists`. Bölüm anahtarı örtük olarak oluşturulmuş `PartitionKey` değeri. 

Aşağıdaki kodu kullanarak tek bir varlık alabilirsiniz:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Daha fazla bilgi için bkz: [tablo API ile geliştirme](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>Gremlin API

Gremlin API ile bir grafik temsil eden bir kapsayıcı oluşturmak için Azure portalında veya Azure CLI kullanabilirsiniz. Alternatif olarak, Azure Cosmos DB çok model olduğundan, diğer API'leri birini oluşturup, grafik kapsayıcı ölçeklendirmek için kullanabilirsiniz.

Kimliği ve bölüm anahtarı Gremlin kullanarak herhangi bir köşe veya kenar okuyabilir. Örneğin, satır anahtarı olarak "Seattle" ve bölüm anahtarı olarak bölgesi ("ABD") ile bir grafik için aşağıdaki sözdizimini kullanarak bir köşe bulabilirsiniz:

```
g.V(['USA', 'Seattle'])
```

Bölüm anahtarını ve satır anahtarını kullanarak bir sınır başvuruda bulunabilir.

```
g.E(['USA', 'I5'])
```

Daha fazla bilgi için bkz: [Azure Cosmos DB'de bölümlenmiş bir grafiğini kullanarak](graph-partitioning.md).


<a name="designing-for-scale"></a>
## <a name="design-for-scale"></a>Ölçek için Tasarım
Etkili bir şekilde Azure Cosmos DB ile ölçeklendirmek için kapsayıcı oluştururken iyi bir bölüm anahtarı seçmeniz gerekir. İyi bir bölüm anahtarı seçme iki ana dikkat edilmesi gereken noktalar vardır:

* **Sorgu sınır ve işlemleri**. Bölüm anahtarı seçiminizi varlıklarınızı ölçeklenebilir bir çözüm sağlamak için birden çok bölüm anahtarları arasında dağıtmak için gereksinim karşı işlemleri kullanmaya gerek dengelemeniz. Bir extreme tüm öğeleri aynı bölüm anahtarı ayarlayabilirsiniz, ancak bu seçenek, çözümünüzün ölçeklenebilirliğini sınırlayabilir. Diğer uçta her öğe için bir benzersiz bir bölüm anahtarı atayabilirsiniz. Bu düzeyde ölçeklenebilir bir seçenektir ancak saklı yordamları ve Tetikleyicileri aracılığıyla belgeler arası işlemleri kullanarak engel olur. İdeal bölüm anahtarı verimli sorguları kullanmanıza olanak tanır ve çözümünüzü ölçeklenebilir olduğundan emin olmak için yeterli kardinalite sahiptir. 
* **Hiçbir depolama ve performans sorunlarını**. Çeşitli farklı değerleri arasında dağıtılacak yazma sağlayan bir özellik seçmek önemlidir. Aynı bölüm anahtarı isteklerini bir bölüm için ayrılan sağlanan işleme aşamaz ve oranı sınırlı olacaktır. Böylece, uygulamanızda "etkin nokta" neden olmayan bir bölüm anahtarı seçmek önemlidir. Tüm veriler tek bölüm anahtarı için bir bölüm içinde saklanmalıdır olduğundan, yüksek miktarda veriyi aynı değere sahip bölüm anahtarlarını kaçınmalısınız. 

Birkaç gerçek dünya senaryoları ve iyi bölüm anahtarlarının her biri için bakalım:
* Bir kullanıcı profili arka uyguluyorsanız *kullanıcı kimliği* bölüm anahtarı için iyi bir seçimdir.
* IOT verileri, örneğin, Aygıt durumu depoluyorsanız bir *cihaz kimliği* bölüm anahtarı için iyi bir seçimdir.
* Zaman serisi veri günlük kaydı için Azure Cosmos DB kullanıyorsanız, *ana bilgisayar adı* veya *kimliği işlem* bölüm anahtarı için iyi bir seçimdir.
* Çok kiracılı bir mimari varsa *kimliği Kiracı* bölüm anahtarı için iyi bir seçimdir.

Bazı durumlarda, IOT ve kullanıcı profilleri gibi kullanmak, aynı bölüm anahtarı olabilir, *kimliği* (belge anahtar). Bazı durumlarda zaman serisi veri gibi farklı bir bölüm anahtarı sahip olabileceğiniz *kimliği*.

### <a name="partitioning-and-loggingtime-series-data"></a>Bölümlendirme ve zaman/günlüğü-serisi veri
Azure Cosmos veritabanı ortak kullanım durumları günlüğe kaydetme ve telemetri biridir. Büyük miktarda veriyi okuma/yazma alacağından Bu senaryoda, iyi bir bölüm anahtarı seçmek önemlidir. Bölüm anahtarı için seçim okuma ve yazma ücretlerinizi ve sorguları çalıştırmak için beklediğiniz türlerini bağlıdır. İyi bir bölüm anahtarı seçmek bazı ipuçları şunlardır:

* Kullanım örneği küçük bir uzun zaman birikmesini yazma oranını içerir ve diğer filtrelerle zaman damgaları aralıklar tarafından sorgulamak ihtiyacınız varsa, zaman damgası toplamını kullanın. Örneğin, iyi bir yaklaşım tarihi bölüm anahtarı olarak kullanmaktır. Bu yaklaşımda, tek bir bölüm belirli bir tarihten tüm verileri sorgulayabilir. 
* İş yükünüzün ise bu senaryoda sık görülür, yazma-ağır zaman damgasını dayalı olmayan bir bölüm anahtarı kullanın. Bu nedenle, Azure Cosmos DB dağıtın ve yazma eşit ölçeklendirme çeşitli bölümleri arasında. Burada bir *ana bilgisayar adı*, *kimliği işlem*, *etkinlik kimliği*, veya başka bir özelliği yüksek önem düzeyi ile iyi bir seçimdir. 
* Burada her gün/ay için birden çok kapsayıcı sahip ve bölüm anahtarı gibi daha ayrıntılı bir özelliği bir karma yaklaşım, başka bir yaklaşımdır *ana bilgisayar adı*. Bu yaklaşım, her kapsayıcı veya kapsayıcıları zaman penceresi ve ölçek ve performans ihtiyaçlarınıza bağlı olarak bir dizi farklı verimlilik ayarlayabilirsiniz avantajına sahiptir. Örneğin, hizmet okuma ve yazma çünkü geçerli ay için bir kapsayıcı daha yüksek bir işleme ile sağlanan. Bunlar yalnızca okuma gördükleri için önceki ay daha düşük bir işleme ile sağlanan.

### <a name="partitioning-and-multitenancy"></a>Bölümlendirme ve çoklu müşteri mimarisi
Azure Cosmos DB kullanarak çok kiracılı bir uygulama uyguluyorsanız, dikkate alınması gereken iki popüler tasarımları vardır: *Kiracı başına bir bölüm anahtarı* ve *Kiracı başına bir kapsayıcı*. Artıları ve eksileri her şunlardır:

* **Kiracı başına bir bölüm anahtarı**. Bu modelde, kiracılar tek bir kapsayıcıda birlikte. Sorgular ve eklemeleri tek bir kiracı için tek bir bölüm karşı gerçekleştirilebilir. İşlem mantığı, bir kiracıya ait olan tüm öğeleri arasında de uygulayabilirsiniz. Birden çok kiracıya bir kapsayıcı paylaştığından, her bir kiracı için sağlama yerine tek bir kapsayıcıdaki tüm kiracılar için kaynak havuzu tarafından depolama ve sağlanan işleme daha iyi kullanabilir. Dezavantajı Kiracı başına performans yalıtımı gerekmemesidir. Performansını garanti etmek için artan işleme için tek bir kiracıya tüm kapsayıcı hedeflenen artar ve tüm kiracılar ile uygulanır.
* **Kiracı başına bir kapsayıcı**. Bu model, her bir kiracı kendi kapsayıcı sahip ve işleme Kiracı başına garantili performansı ile ayırabilirsiniz. Bu birkaç kiracılarla çok müşterili uygulamalar için daha uygun maliyetli modelidir.

Küçük kiracılar birlikte colocates ve büyük kiracılar kendi kapsayıcılara yalıtan bir karma yaklaşım de kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, ölçeklendirme ve Azure Cosmos DB'de bölümleme için bir genel bakış, kavramlar ve en iyi yöntemler sağlanan. 

* Hakkında bilgi edinin [Azure Cosmos veritabanı sağlanan işleme](request-units.md).
* Hakkında bilgi edinin [Azure Cosmos veritabanı genel dağıtım](distribute-data-globally.md).



