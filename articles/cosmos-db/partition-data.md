---
title: Bölümleme ve Azure Cosmos DB'de yatay ölçeklendirme | Microsoft Docs
description: Azure Cosmos DB, bölümleme yapılandırma ve bölüm anahtarları ve uygulamanız için doğru bölüm anahtarı seçmek nasıl bölümleme nasıl çalıştığı hakkında bilgi edinin.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/26/2018
ms.author: andrl
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d7c1c28b3d7b2f51c31f5f05cdef66cc8d71e192
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886393"
---
# <a name="partition-and-scale-in-azure-cosmos-db"></a>Bölümleme ve ölçeklendirme Azure Cosmos DB'de

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) hızlı, öngörülebilir bir performans elde etmenize yardımcı olmak için tasarlanan bir Global olarak dağıtılmış çok modelli veritabanı hizmetidir. Sorunsuz bir şekilde yanı sıra müşterilerinizin uygulamanıza olacak şekilde ölçeklendirir. Bu makalede, Azure Cosmos DB içinde çalıştığı tüm veriler için bölümleme nasıl modeller genel bir bakış sağlar. Ayrıca, uygulamalarınızı etkili bir şekilde ölçeklendirmek için Azure Cosmos DB kapsayıcıları yapılandırma açıklanır.

Azure Cosmos DB aşağıdaki türden kapsayıcıya tüm API'leri destekler:

- **Sabit kapsayıcıyı**: Bu kapsayıcıların bir grafik depolayabilirsiniz 10.000 istek birimi / saniye için ayrılan maksimum boyutu 10 GB'a kadar veritabanı. Sabit bir kapsayıcı oluşturmak için bir bölüm anahtarı özelliği verileri belirtmek gerekli değildir.

- **Sınırsız sayıda kapsayıcı**: Bu kapsayıcıların bir grafiğin yatay bölümleme üzerinden 10 GB sınırından depolamak için otomatik olarak ölçeklendirebilirsiniz. Her bölüm 10 GB depolar ve verileri otomatik olarak bağlı olarak dengelenir **belirtilen bölüm anahtarı**, sınırsız bir kapsayıcıya kullanırken gerekli bir parametre olacak. Bu tür bir kapsayıcı veya saniye başına en fazla 100.000 istek birimleri izin verebilir ve neredeyse sınırsız veri boyutu depolayabilirsiniz [destek ile iletişim kurarak](https://aka.ms/cosmosdbfeedback?subject=Cosmos%20DB%20More%20Throughput%20Request).

## <a name="partitioning-in-azure-cosmos-db"></a>Azure Cosmos DB bölümleme
Azure Cosmos DB koleksiyonları (belgeler için) grafik veya tablo adı verilen verileri depolamak için kapsayıcılar sağlar. Kapsayıcılar mantıksal kaynaklardır ve bir veya daha fazla fiziksel bölüm veya sunucuları yayılabilir. Bölüm sayısı, Azure Cosmos DB depolama boyutuna göre ve kapsayıcı ya da bir dizi kapsayıcı için sağlanan aktarım hızı tarafından belirlenir. 

### <a name="physical-partition"></a>Fiziksel bölüm

A *fiziksel* ayrılmış SSD destekli depolamanın değişken miktarda işlem kaynaklarının (CPU ve bellek) ile birlikte sabit miktarda bölümdür. Her bir fiziksel bölüm, yüksek kullanılabilirlik için çoğaltılır. Kapsayıcılar, her bir kümesi, bir veya daha fazla fiziksel bölüm paylaşabiliriz. Fiziksel bölüm yönetim Azure Cosmos DB tarafından tamamen yönetilir ve karmaşık kod yazın veya bölüm yönetmek zorunda değilsiniz. Azure Cosmos DB kapsayıcıları, depolama ve aktarım hızı bakımından sınırsızdır. Fiziksel bölümler bir iç kavramı Azure Cosmos DB ve geçicidir. Azure Cosmos DB, iş yükünüze bağlı olarak fiziksel bölümlerin sayısını otomatik olarak ölçeklendirir. Bunun yerine fiziksel bölümler sayısına göre veritabanı tasarımı corelate olmamalıdır için mantıksal bölümler belirleyen doğru bölüm anahtarı seçmek emin olmalısınız. 

### <a name="logical-partition"></a>Mantıksal bölüm

A *mantıksal* bir bölüm içinde tek bölüm anahtarı değeri ile ilişkili tüm verileri depolayan bir fiziksel bölüm bölümdür. Birden çok mantıksal bölümler aynı fiziksel bölümünde sonlandırabilirsiniz. Aşağıdaki diyagramda, tek bir kapsayıcıya üç mantıksal bölümü vardır. Her mantıksal bölüm sırasıyla bir bölüm anahtarı, LAX ve AMS MEL verilerini depolar. Her LAX ve AMS MEL mantıksal bölüm 10 GB en yüksek mantıksal bölüm sınırının kuramaz. 

![Kaynak bölümleme](./media/introduction/azure-cosmos-db-partitioning.png) 

Ne zaman bir kapsayıcı karşılayan [önkoşulları bölümleme](#prerequisites), bölümleme uygulamanız için tamamen saydam. Azure Cosmos DB, fiziksel ve mantıksal bölümler arasında veri dağıtımı ve sorgu istekleri için doğru bölüm yönlendirme işler. 

## <a name="how-does-partitioning-work"></a>Bölümleme nasıl çalışır

Her belge olmalıdır bir *bölüm anahtarı* ve *satır anahtarı*, hangi benzersiz şekilde tanımlamak. Bölüm anahtarı, verileriniz için bir mantıksal bölüm görür ve veri fiziksel bölümler arasında dağıtmak amacıyla Azure Cosmos DB ile doğal bir sınır sağlar. **Tek bir mantıksal bölüm için verileri tek bir fiziksel bölüm içinde bulunmalıdır ve fiziksel bölüm yönetim Azure Cosmos DB tarafından yönetilen**. 

Kısaca, işte Azure Cosmos DB'de bölümleme nasıl çalışır:

* Azure Cosmos DB kapsayıcıları ile kümesi sağlama **T** RU/sn (istek / saniye) aktarım hızı.  
* Arka planda, Azure Cosmos DB hizmet için gereken fiziksel bölümler sağlar **T** saniye başına istek sayısı. Varsa **T** en fazla aktarım hızı fiziksel bölüm başına daha yüksek **t**, ardından Azure Cosmos DB hükümlerine **N T/t =** fiziksel bölümler. Azure Cosmos DB tarafından yapılandırılmış partition(t) en fazla üretilen iş değeri, toplam sağlanan aktarım hızı ve kullanılan donanım yapılandırmasını temel alan bu değeri atanır.  
* Azure Cosmos DB bölüm anahtarı alanının boyunca eşit olarak anahtar karmaları ayırır **N** fiziksel bölümler. Bunu, her bir fiziksel bölüm barındırdığı mantıksal bölüm sayısı **1/N** * bölüm anahtarı değerlerine sayısı.  
* Bir fiziksel bölüm olduğunda **p** Azure Cosmos DB, depolama sınırına ulaştığında ayırır, sorunsuz bir şekilde **p** iki yeni fiziksel bölümler halinde **p1** ve **p2**. Bu, her yeni fiziksel bölüm anahtarlarını yaklaşık yarısını karşılık gelen değerleri dağıtır. Bu bölme işlemi, uygulamanızı tamamen görünmez durumdadır. Bir fiziksel bölüm, depolama sınırına ulaştığında ve aynı mantıksal bölüm anahtarına ait tüm veriler fiziksel bir bölüme, bölme işlemi gerçekleşmez. Bu durum, tüm verilerini tek bir mantıksal bölüm anahtarı aynı fiziksel bölümünde bulunmalıdır olmasıdır. Bu durumda, farklı bir bölüm anahtarı stratejisi işe.  
* Daha yüksek aktarım hızı sağlama zaman **t * N**, Azure Cosmos DB, bir veya daha yüksek aktarım hızı desteklemek için fiziksel bölümler böler.

Bölüm anahtarları için anlamları aşağıdaki tabloda gösterildiği gibi her API semantiği eşleştirilecek biraz farklıdır:

| API | Bölüm anahtarı | Satır anahtarı |
| --- | --- | --- |
| SQL | Özel bölüm anahtar yolu | `id` düzeltildi | 
| MongoDB | Özel bir parça anahtarı  | `_id` düzeltildi | 
| Gremlin | Özel bölüm anahtar özelliği | `id` düzeltildi | 
| Tablo | `PartitionKey` düzeltildi | `RowKey` düzeltildi | 

Azure Cosmos DB, karma tabanlı bölümleme kullanır. Bir öğe yazdığınızda, Azure Cosmos DB bölüm anahtarı değerini karma hale getirir ve hangi bölümünün öğeyi depolayacak şekilde belirlemek için karma sonucu kullanır. 

> [!NOTE]
> Azure Cosmos DB, aynı fiziksel bölümünde aynı bölüm anahtarına sahip tüm öğeleri depolar. 

## <a name="best-practices-when-choosing-a-partition-key"></a>Bir bölüm anahtarı seçerken en iyi uygulamalar

Bölüm anahtarı seçimi tasarım zamanında yapmanız önemli bir karardır. Çok çeşitli değerleri ve hatta erişim desenleri sahip bir özellik adı seçin. Çok sayıda farklı değerleri (örneğin, yüzlerce veya binlerce) ile bir bölüm anahtarı için en iyi bir uygulamadır. İş yükünüz bu değerleri arasında eşit bir şekilde dağıtmak olanak tanır. İdeal bir bölüm anahtarı, çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli kardinalite sık sorgularınızı filtre olarak görünür ve biridir.

Bir fiziksel bölüm, depolama sınırına ulaştığında ve bölümdeki veriler aynı bölüm anahtarına sahip, Azure Cosmos DB döndürür *"Bölüm anahtarı, maksimum boyutu 10 GB sınırına"* ileti ve bölüm değil bölün. İyi bir bölüm anahtarı seçmek çok önemli bir karardır. 

Bir bölüm anahtarı seçmek gibi:

* Depolama, hatta tüm anahtarları arasında dağıtımıdır.  
* Verileri bölümler arasında eşit şekilde dağıtacak bölüm anahtarı seçin.

  Verilerinizi bölümler arasında nasıl dağıtıldığını denetlemek için iyi bir fikirdir. Portal veri dağıtımını denetlemek için Azure Cosmos DB hesabınıza gidin ve tıklayarak **ölçümleri** içinde **izleme** bölümüne ve ardından **depolama** görmek için sekmesinde nasıl, verileri farklı fiziksel bölümler arasında bölümlenir.

  ![Kaynak bölümleme](./media/partition-data/partitionkey-example.png)

  Bozuk bölüm anahtarı sonucunu yukarıdaki sol resmi gösterir ve iyi bir bölüm anahtarı seçildi, yukarıdaki sağdaki resimde sonucu gösterir. Soldaki resimde verileri bölümler arasında eşit olarak dağıtılmaz görebilirsiniz. Doğru görüntüye benzer şekilde, verilerinizi dağıtan bir bölüm anahtarı seçmek için çaba göstermelisiniz.

* Mümkün olduğunda bir bölüm sınırları içinde verileri almak için sorguları iyileştirin. Sorgulama desenleri için uygun bir bölümleme stratejisi hizalanabilir. Tek bölümden veri elde sorguları mümkün olan en iyi performansı sağlar. Yüksek eşzamanlılık ile çağrılan sorguları içinde filtre koşulu bölüm anahtarını dahil ederek, verimli bir şekilde yeniden yönlendirilebilir.  

* Daha yüksek bir kardinalite ile bir bölüm anahtarı seçmeyi genellikle tercih edilir: sonuçlanacağını da genellikle daha iyi dağıtımı ve ölçeklenebilirliği verir. Örneğin, bir yapay anahtar kardinalite artırmak için birden çok özellik değerleri ile birleştirerek oluşturulabilir.  

Bölüm anahtarı ile ilgili önemli noktalar yukarıda seçtiğiniz, olarak Azure Cosmos DB fiziksel bölüm sayısının ölçeğini bölümleri veya ne kadar aktarım hızı fiziksel bölüm başına ayrılmış sayısı hakkında endişelenmenize gerek yoktur ve ayrıca ölçeklendirebilirsiniz tek tek bölümler gerektiğinde.

## <a name="prerequisites"></a>Bölümleme için Önkoşullar

Azure Cosmos DB kapsayıcıları sabit olarak oluşturulabilir veya sınırsız. Sabit boyutlu kapsayıcıların üst sınırı 10 GB ve 10.000 RU/sn aktarım hızıdır. Sınırsız olarak bir kapsayıcı oluşturmak için bir bölüm anahtarı ve en düşük aktarım hızı, 1.000 RU/sn belirtmeniz gerekir. Aktarım hızı paylaştıkları gibi Azure Cosmos DB kapsayıcıları da oluşturabilirsiniz. Bu gibi durumlarda, her kapsayıcı specificy gerekir ve bir bölüm anahtarı sınırsız büyüyebilir. 

Bölümleme ve ölçeklendirme için dikkate alınması gereken önkoşullar şunlardır:

* Azure portalında (örneğin, bir koleksiyon, bir grafik veya tablo) bir kapsayıcı oluştururken seçin **sınırsız** sınırsız ölçeklendirme avantajından yararlanmak için depolama kapasitesi seçeneği. Otomatik-bölünmüş fiziksel bölümlere için **p1** ve **p2** açıklandığı [nasıl bölümleme çalışır](#how-does-partitioning-work) makalesi, kapsayıcı bir 1.000 RU/sn aktarım hızı ile oluşturulmalıdır veya Daha fazla (veya bir dizi kapsayıcıları paylaşımı aktarım hızı) ve bölüm anahtarı belirtilmelidir. 

* Büyüktür veya eşittir 1.000 RU/sn ile ilk aktarım hızı bir kapsayıcı oluşturun ve bir bölüm anahtarı sağlayın, sonra kapsayıcınızı herhangi bir değişiklik yapmadan sınırsız ölçeklendirme yararlanabilirsiniz. Başka bir deyişle, oluşturduğunuz olsa bile bir **sabit** kapsayıcı, bir en az 1.000 RU/sn aktarım hızı ile ilk kapsayıcı oluşturulur ve bir bölüm anahtarı belirtilmişse, kapsayıcı bir sınırsız kapsayıcı gibi davranır.

* Aktarım hızı kapsayıcıların kümesinin bir parçası paylaşmak üzere yapılandırılmış tüm kapsayıcıları olarak kabul **sınırsız** kapsayıcıları.

Oluşturduysanız bir **sabit** olmadan kapsayıcı 1. 000'den az RU/sn bölüm anahtarı veya üretilen iş, kapsayıcı otomatik ölçeklendirmeyi olur. Sınırsız bir kapsayıcıya sabit bir kapsayıcıdan verileri geçirmek için kullanmanız gerekir [veri geçiş aracı](import-data.md) veya [değişiklik akışı Kitaplığı](change-feed.md). 

## <a name="designing-for-partitioning"></a> Bir bölüm anahtarı oluşturma 
Kapsayıcı oluşturma ve bunları herhangi bir zamanda ölçeklendirmek için Azure portal veya Azure CLI'yı kullanabilirsiniz. Bu bölümde her API kullanarak sağlanan aktarım hızı ve bölüm anahtarı belirtin ve kapsayıcı oluşturma işlemi gösterilmektedir.


### <a name="sql-api"></a>SQL API’si
Aşağıdaki örnek, SQL API'sini kullanarak bir kapsayıcı (koleksiyon) oluşturma işlemi gösterilmektedir. 

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

Öğesi (belge) kullanarak bir okuma `GET` REST API yöntemini veya kullanarak `ReadDocumentAsync` Sdk'lardan birini içinde.

```csharp
// Read document. Needs the partition key and the ID to be specified
DeviceReading document = await client.ReadDocumentAsync<DeviceReading>(
  UriFactory.CreateDocumentUri("db", "coll", "XMS-001-FE24C"), 
  new RequestOptions { PartitionKey = new PartitionKey("XMS-0001") });
```

Daha fazla bilgi için [SQL API'sini kullanarak Azure Cosmos DB içinde bölümleme](sql-api-partition-data.md).

### <a name="mongodb-api"></a>MongoDB API’si
MongoDB API'si ile en sevdiğiniz aracı, sürücü veya SDK aracılığıyla parçalı bir koleksiyon oluşturabilirsiniz. Bu örnekte, bir koleksiyon oluşturmak için Mongo kabuğunu kullanın.

Mongo Kabuğu'nda:

```
db.runCommand( { shardCollection: "admin.people", key: { region: "hashed" } } )
```
    
Sonuçlar:

```JSON
{
    "_t" : "ShardCollectionResponse",
    "ok" : 1,
    "collectionsharded" : "admin.people"
}
```

### <a name="table-api"></a>Tablo API’si

Tablo API'sini kullanarak tablo oluşturmak için kullanın `CreateIfNotExists` yöntemi. 

```csharp
CloudTableClient tableClient = storageAccount.CreateCloudTableClient();

CloudTable table = tableClient.GetTableReference("people");
table.CreateIfNotExists(throughput: 800);
```

Sağlanan aktarım hızı bağımsız değişken olarak ayarlanmış `CreateIfNotExists`. Bölüm anahtarı örtük olarak oluşturulan `PartitionKey` değeri. 

Aşağıdaki kodu kullanarak tek bir varlığı alabilirsiniz:

```csharp
// Create a retrieve operation that takes a customer entity.
TableOperation retrieveOperation = TableOperation.Retrieve<CustomerEntity>("Smith", "Ben");

// Execute the retrieve operation.
TableResult retrievedResult = table.Execute(retrieveOperation);
```
Daha fazla bilgi için [tablo API'si ile geliştirme](tutorial-develop-table-dotnet.md).

### <a name="gremlin-api"></a>Gremlin API

Gremlin API'si ile bir grafiğini temsil eden bir kapsayıcı oluşturmak için Azure portal veya Azure CLI'yı kullanabilirsiniz. Alternatif olarak, Azure Cosmos DB çok modelli olduğundan, diğer API'lerden birini oluşturacağınız ve ölçeklendireceğiniz, grafik kapsayıcısı için kullanabilirsiniz.

> [!NOTE]
> Kullanamazsınız `/id` Gremlin API içinde bir kapsayıcı için bölüm anahtarı olarak. 

Bölüm anahtarına ve Kimliğine Gremlin kullanarak, herhangi bir köşe veya kenarın okuyabilirsiniz. Örneğin, bölüm anahtarını ve satır anahtarı olarak "Seattle" olarak ("ABD") bölgesi olan bir grafik için aşağıdaki sözdizimini kullanarak, bir köşe bulabilirsiniz:

```
g.V(['USA', 'Seattle'])
```

Bir edge bölüm anahtarını ve satır anahtarı kullanarak başvurabilirsiniz.

```
g.E(['USA', 'I5'])
```

Daha fazla bilgi için [bölümlenmiş bir grafik kullanarak Azure Cosmos DB'de](graph-partitioning.md).

## <a name="form-partition-key-by-concatenating-multiple-fields"></a>Birden çok alan birleştirerek tarafından form bölüm anahtarı

Bir bölüm anahtarı, birleştirme ve bir öğesinin tek yapay "partitionKey" özelliği birden çok özellik değerlerini doldurma de oluşturabilir. Bu anahtarları yapay anahtarlar denir.

Örneğin, şuna benzer bir belge vardır:

```json
{
"deviceId": "abc-123",
"date": 2018
}
```

PartitionKey /deviceId veya /date ayarlamak bir seçenektir. Cihaz kimliği ve tarihi bir bölüm anahtarı oluşturmak istiyorsunuz. Yapay "partitionKey" özelliği için bu iki değerleri birleştirmek ve /partitionKey için bölüm anahtarını ayarlayın.

```json
{
"deviceId": "abc-123",
"date": 2018,
"partitionKey": "abc-123-2018"
}
```

Yapay bir anahtara değerleri birleştirebilir, yapay anahtar belgelerine ekleme ve bölüm anahtarı belirtmek için kullanın, istemci tarafı mantığı tanımlamalıdır. Bu nedenle, gerçek zamanlı senaryolarda belgeleri binlerce olabilir.

## <a name="designing-for-scale"></a> Ölçek için Tasarım
Azure Cosmos DB ile etkili bir şekilde ölçeklendirmek için kapsayıcı oluşturmak iyi bir bölüm anahtarı seçmek gerekir. İyi bir bölüm anahtarı seçmek için iki ana hususlar vardır:

* **Sorgu sınır ve işlemleri**. Seçtiğiniz bölüm anahtarına varlıklarınızı ölçeklenebilir bir çözüm sağlamak için birden çok bölüm anahtarları arasında dağıtmak için gereksinim işlemler kullanmaya gerek dengelemeniz. Bir üst düzey, tüm öğeler için aynı bölüm anahtarı ayarlayabilirsiniz, ancak bu seçenek, çözümünüzün ölçeklenebilirliği sınırlayabilir. Diğer uçta her öğesi için benzersiz bir bölüm anahtarı atayabilirsiniz. Bu yüksek oranda ölçeklenebilir bir seçenektir, ancak saklı yordamları ve Tetikleyicileri aracılığıyla belgeler arası işlemleri kullanarak engel olur. İdeal bir bölüm anahtarı, verimli sorgular kullanmanıza olanak tanır ve çözümünüzün ölçeklenebilir olduğundan emin olmak için yeterli kardinalite sahiptir. 
* **Hiçbir depolama ve performans sorunlarını**. Yazma çeşitli farklı değerleri arasında dağıtılmasını sağlayan bir özelliği seçmek önemlidir. Aynı bölüm anahtarına isteklerini ayrılan bir bölümü için sağlanan aktarım hızı aşamaz ve oranı sınırlı olur. Bu nedenle uygulamanız içinde "etkin nokta" neden olmayan bir bölüm anahtarı seçmek önemlidir. Tüm verilerin tek bir bölüm anahtarı için bir bölüm içinde depolanmış olması gerekir çünkü yüksek hacimli verileri için aynı değere sahip bir bölüm anahtarları kaçınmanız gerekir. 

Şimdi birkaç gerçek dünya senaryoları ve iyi bölüm anahtarları her biri için bakın:
* Bir kullanıcı profili arka ucu uyguluyorsanız *kullanıcı kimliği* bir bölüm anahtarı için iyi bir seçimdir.
* IOT verilerini, örneğin, cihaz durumu depoluyorsanız bir *cihaz kimliği* bir bölüm anahtarı için iyi bir seçimdir.
* Azure Cosmos DB için zaman serisi verilerini günlüğe kullanıyorsanız *hostname* veya *kimliği işlem* bir bölüm anahtarı için iyi bir seçimdir.
* Bir çok kiracılı mimari varsa *Kiracı kimliği* bir bölüm anahtarı için iyi bir seçimdir.

Bazı durumlarda, IOT ve kullanıcı profilleri gibi kullanın, bölüm anahtarı olarak aynı olabilir, *kimliği* (belge anahtarını). Bazı durumlarda, zaman serisi verileri gibi farklı bir bölüm anahtarı sahip olabileceğiniz *kimliği*.

### <a name="partitioning-and-loggingtime-series-data"></a>Bölümleme ve günlüğe kaydetme/zaman serisi verileri
Azure Cosmos DB ortak kullanım örneklerini günlüğe kaydetme ve telemetri biridir. Büyük hacimli verileri okuma/yazma ihtiyacınız olabilecek çünkü bu senaryoda, iyi bir bölüm anahtarı seçmek önemlidir. Bir bölüm anahtarı seçimi, okuma ve yazma oranları ve sorguları çalıştırmak için beklediğiniz türlerini bağlıdır. İyi bir bölüm anahtarı seçmek bazı ipuçları şunlardır:

* Kullanım Örneğinize küçük bir uzun bir zaman içinde birikmesini yazma oranını içerir ve zaman damgaları aralıklarına göre diğer filtrelerle sorgulamak ihtiyacınız varsa, zaman damgası bir toplama kullanın. Örneğin, iyi bir yaklaşım tarih bir bölüm anahtarı olarak kullanmaktır. Bu yaklaşımda, tek bir bölüm belirli bir tarihten tüm verileri sorgulayabilirsiniz. 
* Bu senaryoda çok yaygın olan yazma-ağır iş yükünüz, zaman damgasını dayalı olmayan bir bölüm anahtarı kullanın. Bu nedenle, Azure Cosmos DB dağıtın ve yazma eşit bir şekilde ölçeklendirme çeşitli bölümler arasında. Burada bir *hostname*, *kimliği işlem*, *etkinlik kimliği*, veya başka bir özelliğiyle yüksek kardinalite iyi bir seçimdir. 
* Burada için her gün/ay, birden çok kapsayıcı olması ve bölüm anahtarı gibi daha ayrıntılı bir özelliğin karma bir yaklaşım, başka bir yaklaşımdır *hostname*. Bu yaklaşım, her kapsayıcı ya da kapsayıcıları zaman penceresi ve ölçek ve performans ihtiyaçlarınıza göre bir dizi için farklı bir aktarım hızı ayarlayabileceğiniz avantajına sahiptir. Örneğin, hizmet okur ve yazar çünkü geçerli ay için bir kapsayıcı daha yüksek bir aktarım hızı ile sağlanan. Bunlar yalnızca okuma gördükleri için önceki aylarda daha düşük bir aktarım hızı ile sağlanan.

### <a name="partitioning-and-multitenancy"></a>Bölümlendirme ve çoklu müşteri mimarisi
Azure Cosmos DB kullanarak çok kiracılı bir uygulama uyguluyorsanız, dikkate alınması gereken iki popüler tasarımları vardır: *Kiracı başına bir bölüm anahtarı* ve *Kiracı başına bir kapsayıcı*. Artıları ve eksileri her şunlardır:

* **Kiracı başına bir bölüm anahtarı**. Bu modelde, kiracılar tek bir kapsayıcıda birlikte. Sorgular ve tek bir kiracının ekler, tek bir bölüm karşı gerçekleştirilebilir. Ayrıca, bir kiracıya ait olan tüm öğeleri arasında işlem mantığı uygulayabilir. Birden çok kiracıya bir kapsayıcı paylaştığından, her Kiracı için sağlama yerine tek bir kapsayıcı içindeki tüm kiracılar için kaynak havuzu tarafından depolama ve sağlanan aktarım hızı daha iyi kullanabilir. Dezavantajı Kiracı başına performans yalıtımı gerekmemesidir. Performansını garanti etmek için artan bir işleme hacmi varsayılan olarak, tek bir kiracı için tüm kapsayıcı ile hedeflenen artar ve tüm kiracılar için uygulanır.
* **Kiracı başına bir kapsayıcı**. Bu model, her kiracının kendi kapsayıcı vardır ve garantili performans Kiracı başına aktarım hızı ayırabilirsiniz. Bu model birkaç kiracılar ile çok kiracılı uygulamalar için daha uygun maliyetlidir.

Küçük kiracılar birlikte colocates ve daha büyük kiracılar kendi kapsayıcılarına yalıtır karma bir yaklaşım da kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, Azure Cosmos DB'de bölümleme ve ölçeklendirme için temel kavramlar ve en iyi uygulamalar hakkında genel bir bakış sağlanır. 

* Hakkında bilgi edinin [Azure Cosmos DB'de sağlanan aktarım hızı](request-units.md).
* Hakkında bilgi edinin [Azure Cosmos DB'de global dağıtım](distribute-data-globally.md).



