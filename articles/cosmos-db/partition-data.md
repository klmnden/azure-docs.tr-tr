---
title: "Bölümlendirme ve yatay Azure Cosmos DB'de ölçekleme | Microsoft Docs"
description: "Azure Cosmos DB, bölümleme yapılandırmak ve anahtarları bölümlemek nasıl ve uygulamanız için doğru bölüm anahtarı almak nasıl bölümleme nasıl çalıştığı hakkında bilgi edinin."
services: cosmos-db
author: arramac
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: cac9a8cd-b5a3-4827-8505-d40bb61b2416
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2018
ms.author: arramac
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0032a00883cedfe754e14293dc13a1009f6dd3a0
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/05/2018
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Bölüm ve ölçek Azure Cosmos veritabanı

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hızlı ve tahmin edilebilir bir performans elde yardımcı olmak için tasarlanmış bir genel dağıtılmış, multimodel veritabanı hizmetidir. Bunu büyüdükçe yanı sıra, uygulamanızın sorunsuz bir şekilde ölçeklendirir. Bu makalede Azure Cosmos DB içinde çalıştığı tüm veriler için bölümleme nasıl modeller genel bir bakış sağlar. Ayrıca, uygulamalarınızı etkili bir şekilde ölçeklendirmek için Azure Cosmos DB kapsayıcıları nasıl yapılandırabileceğiniz anlatılmaktadır.

Bölümleme ve bölüm anahtarlarını bu Azure Cuma Scott Hanselman ve Azure Cosmos DB sorumlu mühendislik Yöneticisi, Shireesh Thota ile video ele alınmaktadır:

> [!VIDEO https://channel9.msdn.com/Shows/Azure-Friday/Azure-DocumentDB-Elastic-Scale-Partitioning/player]
> 

## <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB bölümlendirme
Azure Cosmos DB'de depolamak ve herhangi bir ölçekte milisaniyelik sipariş yanıt süreleri ile şema daha az veri sorgulayabilirsiniz. Azure Cosmos DB veri depolama adı verilen kapsayıcıları sağlar *koleksiyonları* (belgeler için), *grafikleri*, veya *tabloları*. 

Kapsayıcıları mantıksal kaynaklar ve bir veya daha fazla fiziksel bölümleri veya sunucuları yayılabilir. Bölüm sayısı Azure Cosmos depolama boyutu ve kapsayıcının sağlanan işleme dayalı DB tarafından belirlenir. 

Ayrılmış SSD yedekli depolama sabit miktarlı bir fiziksel bölümdür. Her fiziksel bölüm yüksek kullanılabilirlik için çoğaltılır. Bir veya daha fazla fiziksel bölüm kapsayıcısı ayarlama olun. Fiziksel bölüm yönetimi tam olarak Azure Cosmos DB tarafından yönetilir ve karmaşık kodlar yazmak veya bölüm yönetmek yok. Azure Cosmos DB depolama ve işleme açısından sınırsız kapsayıcılardır. 

Bir mantıksal bölüm tek bölüm anahtar değeriyle ilişkili tüm verileri depolar fiziksel bir bölüm içinde bir bölümdür. Bir mantıksal bölüm 10 GB en sahiptir. Aşağıdaki diyagramda, tek bir kapsayıcı üç mantıksal bölümler vardır. Her mantıksal bölüm için bir bölüm anahtarı, LAX, AMS ve MEL sırasıyla depolamaz. Her LAX, AMS ve MEL mantıksal bölüm 10 GB en büyük mantıksal bölüm sınırı aşan kuramaz. 

![Kaynak bölümlendirme](./media/introduction/azure-cosmos-db-partitioning.png) 

Bir koleksiyonun ne zaman karşılayan [Önkoşullar bölümleme](#prerequisites), bölümlendirme işlemi uygulamanız için saydamdır. Azure Cosmos DB hızlı okuma ve yazma, sorguları, işlem mantığı, tutarlılık düzeyleri ve ayrıntılı erişim denetimi yöntemlerini/API'leri tek kapsayıcı kaynağa yoluyla destekler. Verileri fiziksel ve mantıksal bölümleri arasında dağıtma ve yönlendirme hizmeti tanıtıcıları sağ bölüm isteklerine sorgu. 

## <a name="how-does-partitioning-work"></a>Bölümleme nasıl çalışır

Bölümleme nasıl çalışır? Her bir öğeyi benzersiz olarak tanımlamak bölüm anahtarı ve bir satır anahtarı olması gerekir. Bölüm anahtarı, verileriniz için bir mantıksal bölüm görevi görür ve fiziksel bölümler veri dağıtılmasında doğal bir sınır ile Azure Cosmos DB sağlar. Tek bir mantıksal bölüm için verileri tek bir fiziksel bölüm içinde bulunmalıdır, ancak fiziksel bölüm yönetimi Azure Cosmos DB tarafından yönetilen unutmayın. 

Kısaca, işte Azure Cosmos DB'de bölümleme nasıl çalışır:

* Bir Azure Cosmos DB kapsayıcıyla sağlamak **T** ikinci üretilen iş başına istek sayısı.
* Arka planda Azure Cosmos DB sunmak için gereken bölümleri sağlar **T** saniye başına istek sayısı. Varsa **T** bölüm başına en fazla üretilen daha yüksek **t**, ardından Azure Cosmos DB hükümleri **N T/t =** bölümler.
* Azure Cosmos DB anahtar alanı bölümünün eşit yatay anahtar karmaları ayırır **N** bölümler. Bunu, her bölüm (fiziksel bölüm) konakları **1/N** bölüm anahtarı değerlerini (mantıksal bölümler).
* Fiziksel bir bölüm olduğunda **p** Azure Cosmos DB, depolama sınırına ulaştığında sorunsuz bir şekilde böler **p** iki yeni bölümlere, **p1** ve **p2** . Her bölüm için kabaca yarım anahtarlara karşılık gelen değerleri dağıtır. Bu işlemi bölünmüş uygulamanıza görünmez durumdadır. Fiziksel bir bölüm, depolama sınırına ulaştığında ve aynı mantıksal bölüm anahtarına ait tüm fiziksel bölümündeki verileri bölme işlemi gerçekleşmez. Tek bir mantıksal bölüm anahtarı için tüm veriler aynı fiziksel bölümünde yer alması ve fiziksel bölüm p1 ve p2 böylece bölünemez nedeni budur. Bu durumda farklı bölüm anahtar stratejisi işe.
* Daha yüksek verimlilik sağlamak zaman  **t*N**, bir veya daha yüksek verimlilik desteklemek için bölümler Azure Cosmos DB böler.

Bölüm anahtarlarını anlamları aşağıdaki tabloda gösterildiği gibi her API semantiği eşleşmesi biraz farklılık gösterir:

| API | Bölüm anahtarı | Satır anahtarı |
| --- | --- | --- |
| Azure Cosmos DB | Özel bölüm anahtar yolu | `id` düzeltildi | 
| MongoDB | Özel paylaşılan anahtar  | `_id` düzeltildi | 
| Graf | Özel bölüm anahtar özelliği | `id` düzeltildi | 
| Tablo | `PartitionKey` düzeltildi | `RowKey` düzeltildi | 

Azure Cosmos DB karma tabanlı bölümleme kullanır. Bir öğe yazdığınızda, Azure Cosmos DB bölüm anahtarı değerini karma hale getirir ve karma hale getirilen sonuç öğesinde depolamak için hangi bölümünü belirlemek için kullanır. Azure Cosmos DB tüm öğeleri aynı fiziksel bölümünde aynı bölüm anahtarına sahip depolar. Bölüm anahtarı seçimi tasarım zamanında yapmak zorunda önemli bir karardır. Çok çeşitli değerleri ve hatta erişim desenlerini sahip bir özellik adı seçmeniz gerekir. Fiziksel bir bölüm, depolama sınırına ulaştığında ve bölümdeki tüm verileri aynı bölüm anahtarına açıktır, Azure Cosmos DB "Bölüm anahtarı 10 GB boyut sınırına ulaşmış" hatası döndürür ve bölüm, böylece bir bölüm anahtarı seçme bölünen değil çok alma ise ant karar.

> [!NOTE]
> Birçok farklı değerleri (yüz binlerce en az) sahip bir bölüm anahtarı için en iyi bir uygulamadır.
>

Azure Cosmos DB kapsayıcılar olarak oluşturulabilir *sabit* veya *sınırsız* Azure portalında. Sabit boyutlu kapsayıcıları 10 GB ve 10. 000'ru / s işleme üst sınırına sahip. Sınırsız olarak bir kapsayıcı oluşturmak için en düşük işleme 1.000 RU/s belirtin ve bir bölüm anahtarı belirtmeniz gerekir.

Verilerinizi bölümlerinde nasıl dağıtıldığını denetlemek için iyi bir fikirdir. Bu portalda denetlemek için Azure Cosmos DB hesabınıza gidin ve tıklayın **ölçümleri** içinde **izleme** bölümünde ve ardından Sağdaki bölmede üzerinde **depolama** verilerinizi nasıl olduğunu görmek için sekmesi farklı fiziksel bölümünde bölümlenmiş.

![Kaynak bölümlendirme](./media/partition-data/partitionkey-example.png)

Bozuk bölüm anahtarı sonucunu sol görüntü gösterir ve sağ görüntü iyi bir bölüm anahtarı sonucunu gösterir. Sol görüntüde veri bölümleri arasında eşit olarak dağıtılmaz görebilirsiniz. Grafik sağ görüntüsüne benzer şekilde, verilerinizi dağıtmak çalışmalarımızı.

<a name="prerequisites"></a>
## <a name="prerequisites-for-partitioning"></a>Bölümleme için Önkoşullar

Fiziksel bölümler için otomatik Böl **p1** ve **p2** açıklandığı gibi [nasıl bölümleme çalışır](#how-does-partitioning-work), kapsayıcı 1.000 RU/s verimini veya daha fazla ile oluşturulması gerekir , ve bir bölüm anahtarı sağlanmalıdır. Bir kapsayıcı Azure portalında oluşturulurken seçin **sınırsız** bölümlendirme ve otomatik ölçeklendirme yararlanmak için depolama kapasitesi seçeneği. 

Azure portal veya program aracılığıyla kapsayıcı oluşturuldu ve ilk verimlilik 1.000 RU/s ya da daha fazla ve verilerinizi bir bölüm anahtarı içerir, herhangi bir değişiklik, kapsayıcı ile bölümleme avantajından yararlanmak - bu içerir, **sabit**  ilk kapsayıcı througput içinde en az 1.000 RU/s ile oluşturulduğundan ve bölüm anahtarı verilerde yok sürece kapsayıcıları, boyut.

Oluşturduysanız bir **sabit** anahtar ya da oluşturulan hiçbir bölüm boyutu kapsayıcıyla bir **sabit** boyutu kapsayıcı 1.000 RU/s'den küçük üretilen iş ile kapsayıcı olamaz otomatik-bölünmüş bu makalede anlatıldığı gibi. Bu gibi kapsayıcı verileri sınırsız bir kapsayıcı (bir işleme ve bölüm anahtarı en az 1.000 RU/s) geçirmek için kullanmanız gerekir [veri geçiş aracı](import-data.md) veya [değişiklik akış Kitaplığı](change-feed.md) için değişiklikleri geçirin. 

## <a name="partitioning-and-provisioned-throughput"></a>Bölümlendirme ve sağlanan işleme
Azure Cosmos DB tahmin edilebilir performans için tasarlanmıştır. Bir kapsayıcı oluşturduğunuzda, üretilen iş açısından, yedek  *[istek birimleri](request-units.md) (RU) saniye başına*. Her istek CPU, bellek ve g/ç işlemi tarafından tüketilen gibi sistem kaynaklarının miktarını orantılıdır RU ücret atanır. Oturum tutarlılığı 1 KB belgeyle okuma 1 tüketir RU. Okuma 1. RU bağımsız olarak depolanan öğeler sayısını veya aynı anda çalıştırılması eşzamanlı istek sayısı. Büyük öğeleri boyutuna bağlı olarak daha yüksek RUs gerektirir. Varlıklarınızı ve uygulamanız için desteklemeniz gereken okuma sayısını boyutunu biliyorsanız, verimlilik ihtiyaçlarını okuma, uygulama için gerekli miktarda sağlayabilirsiniz. 

> [!NOTE]
> Kapsayıcı tam verimini elde etmek için istekleri bazı farklı bölüm anahtar değerleri arasında eşit olarak dağıtmanızı sağlayan bir bölüm anahtarı seçmeniz gerekir.
> 
> 

<a name="designing-for-partitioning"></a>
## <a name="work-with-the-azure-cosmos-db-apis"></a>Azure Cosmos DB API'leri ile çalışma
Kapsayıcılar oluşturmak ve bunları herhangi bir anda ölçeklendirmek için Azure portalında veya Azure CLI kullanın. Bu bölümde, kapsayıcıları oluşturma ve üretilen iş ve bölüm anahtar tanımını her desteklenen API'ları belirtin gösterilmektedir.

### <a name="azure-cosmos-db-api"></a>Azure Cosmos DB API
Aşağıdaki örnek, Azure Cosmos DB API'sini kullanarak bir kapsayıcı (toplama) oluşturulacağını gösterir. 

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

Kullanarak bir öğe (belge) okuyabilirsiniz `GET` yöntemi REST API veya kullanarak `ReadDocumentAsync` SDK'lar birinde.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

### <a name="mongodb-api"></a>MongoDB API’si
MongoDB API'si ile sık kullanılan aracı, sürücü veya SDK aracılığıyla parçalı bir koleksiyon oluşturabilirsiniz. Bu örnekte koleksiyonu oluşturma işleminde Mongo kabuğunu kullanırız.

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

Azure Cosmos DB tablo API'sini kullanarak bir tablo oluşturmak için CreateIfNotExists yöntemini kullanın. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Üretilen iş CreateIfNotExists bağımsız değişken olarak ayarlanır.

Bölüm anahtarı örtük olarak oluşturulmuş `PartitionKey` değeri. 

Aşağıdaki kod parçacığını kullanarak tek bir varlık alabilirsiniz:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Daha fazla bilgi için bkz: [tablo API ile geliştirme](tutorial-develop-table-dotnet.md).

### <a name="graph-api"></a>Graph API

Grafik API'si ile kapsayıcı oluşturmak için Azure portalında veya Azure CLI kullanmalısınız. Alternatif olarak, Azure Cosmos DB multimodel olduğundan, bir başka modellerinin oluşturmak ve grafik kapsayıcı ölçeklendirmek için kullanabilirsiniz.

Kimliği ve bölüm anahtarı Gremlin kullanarak herhangi bir köşe veya kenar okuyabilir. Örneğin, satır anahtarı olarak "Seattle" ve bölüm anahtarı olarak bölgesi ("ABD") ile bir grafik için aşağıdaki sözdizimini kullanarak bir köşe bulabilirsiniz:

```
g.V(['USA', 'Seattle'])
```

Bölüm anahtarını ve satır anahtarını kullanarak bir sınır başvuruda bulunabilir.

```
g.E(['USA', 'I5'])
```

Daha fazla bilgi için bkz: [Azure Cosmos DB Gremlin desteği](gremlin-support.md).


<a name="designing-for-partitioning"></a>
## <a name="design-for-partitioning"></a>Bölümleme için tasarımı
Etkili bir şekilde Azure Cosmos DB ile ölçeklendirmek için kapsayıcı oluştururken iyi bir bölüm anahtarı seçmeniz gerekir. Bölüm anahtarı seçme iki ana dikkat edilmesi gereken noktalar vardır:

* **Sorgu ve işlemler için sınır**. Bölüm anahtarı seçiminizi varlıklarınızı ölçeklenebilir bir çözüm sağlamak için birden çok bölüm anahtarları arasında dağıtmak için gereksinim karşı işlemleri kullanımını etkinleştirmek için gereken dengelemeniz. Bir extreme tüm öğeleri aynı bölüm anahtarı ayarlayabilirsiniz, ancak bu seçenek, çözümünüzün ölçeklenebilirliğini sınırlayabilir. Diğer uçta her öğe için bir benzersiz bir bölüm anahtarı atayabilirsiniz. Bu düzeyde ölçeklenebilir bir seçenektir ancak saklı yordamları ve Tetikleyicileri aracılığıyla belgeler arası işlemleri kullanarak engel olur. İdeal bölüm anahtarı verimli sorguları kullanmanıza olanak tanır ve çözümünüzü ölçeklenebilir olduğundan emin olmak için yeterli kardinalite sahiptir. 
* **Hiçbir depolama ve performans sorunlarını**. Çeşitli farklı değerleri arasında dağıtılacak yazma sağlayan bir özellik seçmek önemlidir. Aynı bölüm anahtarı isteklerini tek bir bölüm verimini aşamaz ve kısıtlanan. Böylece, uygulamanızda "etkin nokta" neden olmayan bir bölüm anahtarı seçmek önemlidir. Tüm veriler tek bölüm anahtarı için bir bölüm içinde saklanmalıdır olduğundan, yüksek miktarda veriyi aynı değere sahip bölüm anahtarlarını kaçınmalısınız. 

Birkaç gerçek dünya senaryoları ve iyi bölüm anahtarlarının her biri için bakalım:
* Bir kullanıcı profili arka uç uyguluyorsanız kullanıcı kimliği bölüm anahtarı için iyi bir seçimdir.
* IOT verileri, örneğin, Aygıt durumu depoluyorsanız bir cihaz kimliği bölüm anahtarı için iyi bir seçimdir.
* Zaman serisi veri günlük kaydı için Azure Cosmos DB kullanıyorsanız, ana bilgisayar adı veya işlem kimliği bölüm anahtarı için iyi bir seçimdir.
* Çok kiracılı bir mimari varsa, Kiracı kimliği bölüm anahtarı için iyi bir seçimdir.

Bazı durumlarda, IOT ve kullanıcı profilleri gibi kullanım, bölüm anahtarı, Kimliğinizi (belge anahtarı) ile aynı olması. Bazı durumlarda zaman serisi veri gibi kimliğinden farklı bir bölüm anahtarı olabilir

### <a name="partitioning-and-loggingtime-series-data"></a>Bölümlendirme ve zaman/günlüğü-serisi veri
Azure Cosmos DB'nin ortak kullanım durumları günlüğe kaydetme ve telemetri biridir. Büyük miktarda veriyi okuma/yazma alacağından iyi bir bölüm anahtarı, çekme önemlidir. Seçimi okuma ve yazma ücretlerinizi ve sorguları çalıştırmak için beklediğiniz türlerini bağlıdır. İyi bir bölüm anahtarı seçmek bazı ipuçları şunlardır:

* Kullanım örneği küçük bir uzun zaman birikmesini yazma oranını içerir ve zaman damgaları ve diğer filtreleri aralıklarına göre sorgulamak ihtiyacınız varsa, zaman damgası toplamını kullanın. Örneğin, iyi bir yaklaşım tarihi bölüm anahtarı olarak kullanmaktır. Bu yaklaşımda, tek bir bölüm tarihinden tüm verileri sorgulayabilir. 
* İş yükünüzün daha yaygın bir durumdur, ağır yazılmışsa zaman damgası dayalı olmayan bir bölüm anahtarı kullanın. Bu yaklaşımda, Azure Cosmos DB yazma eşit çeşitli bölümler dağıtabilirsiniz. Burada bir ana bilgisayar adı, işlem kimliği, etkinlik kimliği veya başka bir özelliği yüksek önem düzeyi ile iyi bir seçimdir. 
* Burada her gün/ay için birden çok kapsayıcı sahip ve bölüm anahtarı ana bilgisayar adı gibi ayrıntılı bir özelliği bir karma bir başka bir yaklaşımdır. Bu yaklaşım, zaman penceresi göre farklı verimlilik ayarlayabilirsiniz avantajına sahiptir. Örneğin, hizmet okuma ve yazma çünkü geçerli ay için kapsayıcı daha yüksek işleme ile sağlanır. Bunlar yalnızca okuma gördükleri için önceki ay daha düşük işleme ile sağlanır.

### <a name="partitioning-and-multitenancy"></a>Bölümlendirme ve çoklu müşteri mimarisi
Azure Cosmos DB kullanarak çok kiracılı uygulama uyguluyorsanız, iki popüler modeli vardır: Kiracı ve Kiracı başına bir kapsayıcı her bir bölüm anahtarı. Artıları ve eksileri her şunlardır:

* **Kiracı başına bir bölüm anahtarı**. Bu modelde, kiracılar tek bir kapsayıcıda birlikte bulunan. Ancak, sorgular ve eklemeleri tek bir kiracı içinde öğeleri için tek bir bölüm karşı gerçekleştirilebilir. İşlem mantığı, bir kiracı içindeki tüm öğeler arasında de uygulayabilirsiniz. Birden çok kiracıya bir kapsayıcı paylaştığından, her bir kiracı için ek boş alan sağlama yerine tek bir kapsayıcıdaki kiracılar için kaynak havuzu tarafından depolama ve işleme maliyetleri kaydedebilirsiniz. Dezavantajı Kiracı başına performans yalıtımı gerekmemesidir. Performans/verimliliği artırır kapsayıcının tamamı hedeflenen artar ve kiracılar için geçerlidir.
* **Kiracı başına bir kapsayıcı**. Bu model, her bir kiracı kendi kapsayıcı sahip ve Kiracı başına performans ayırabilirsiniz. Azure Cosmos yeni fiyatlandırma sağlama DB ile bu birkaç kiracılarla çok müşterili uygulamalar için daha uygun maliyetli modelidir.

Ayrıca, küçük kiracılar collocates ve büyük kiracılar kendi kapsayıcıya Geçiren birleşimi ve katmanlı bir yaklaşım kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, tüm Azure Cosmos DB API'si ile bölümleme için biz kavramları ve en iyi yöntemler genel bakış sağlanır. 

* Hakkında bilgi edinin [Azure Cosmos veritabanı sağlanan işleme](request-units.md).
* Hakkında bilgi edinin [Azure Cosmos veritabanı genel dağıtım](distribute-data-globally.md).



