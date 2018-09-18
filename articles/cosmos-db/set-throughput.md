---
title: Azure Cosmos DB için sağlama aktarım hızı | Microsoft Docs
description: Azure Cosmos DB containsers, koleksiyonlar, grafikler ve tablolar için sağlanan aktarım hızı ayarlama konusunda bilgi edinin.
services: cosmos-db
author: aliuy
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: andrl
ms.openlocfilehash: 2da00f700f5cc234455cc686377e5863f1c35bdd
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/17/2018
ms.locfileid: "45734480"
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Azure Cosmos DB kapsayıcıları ve veritabanı için aktarım hızı alma ve ayarlama

Azure portalını kullanarak veya istemci SDK'ları kullanarak, bir Azure Cosmos DB kapsayıcısı veya bir dizi kapsayıcıları için aktarım hızı ayarlayabilirsiniz. 

**Bir kapsayıcının aktarım hızını sağlama:** kapsayıcıları kümesi için aktarım hızı sağladığınızda, sağlanan aktarım hızı tüm kapsayıcıların paylaşın. Tek tek kapsayıcılar için sağlama aktarım hızı, belirli bir kapsayıcı için aktarım hızının ayırma garanti eder. Olarak RU/sn tek tek kapsayıcı düzeyinde atarken kapsayıcıları oluşturulabilir *sabit* veya *sınırsız*. Sabit boyutlu kapsayıcıların üst sınırı 10 GB ve 10.000 RU/sn aktarım hızıdır. Sınırsız bir kapsayıcı oluşturmak için en düşük aktarım hızı, 1.000 RU/sn belirtmeniz gerekir ve bir [bölüm anahtarı](partition-data.md). Verilerinizi birden çok bölümler arasında bölünmesi gerekebilir olduğundan, yüksek bir kardinalite (100 milyonlarca ayrı değer) sahip bir bölüm anahtarı seçmek gereklidir. Birçok farklı değerlere sahip bir bölüm anahtarı'nı seçerek, kapsayıcı/tablo/grafik ve isteklerini aynı şekilde Azure Cosmos DB tarafından ölçeklendirilebilir emin olun. 

**Bir kapsayıcı veya bir veritabanı kümesinin sağlama aktarım hızı:** sağlama aktarım hızı bir veritabanı için aktarım hızı o veritabanına ait tüm kapsayıcıları arasında paylaşmanızı sağlar. Bir Azure Cosmos DB veritabanında, kapsayıcılar, adanmış aktarım hızı yanı sıra aktarım hızını paylaşır kapsayıcıları kümesi olabilir. Bu kümeye ait olan kapsayıcıları RU/sn kapsayıcıları kümesi atarken değerlendirilir *sınırsız* kapsayıcılar ve bölüm anahtarı belirtmeniz gerekir.

Sağlanan aktarım hızına göre Azure Cosmos DB büyüdükçe, kapsayıcılar ve Gruplama/rebalances verilerinizi bölümler arasında barındırmak için fiziksel bölümlere ayırır. Kapsayıcı ve veritabanı düzeyi aktarım hızı sağlama ayrı teklifleri ve ya da bunların arasında geçiş gerektiren geçirme kaynaktan hedef veri. Yeni bir veritabanı veya yeni bir koleksiyon oluşturun ve ardından verileri kullanarak geçirmek için anlamına gelir [toplu Yürütücü Kitaplığı](bulk-executor-overview.md) veya [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md). Aşağıdaki görüntüde, farklı düzeylerde sağlama aktarım hızı gösterilmiştir:

![İstek birimleri için ayrı kapsayıcıları ve kapsayıcıları kümesi sağlama](./media/request-units/provisioning_set_containers.png)

Sonraki bölümlerde, aktarım hızı, Azure Cosmos DB hesabı için farklı düzeylerde yapılandırmak için gereken adımları öğreneceksiniz. 

## <a name="provision-throughput-by-using-azure-portal"></a>Azure portalını kullanarak hazırlama aktarım hızı

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Bir kapsayıcı (koleksiyon/grafik/tablo) için sağlama aktarım hızı

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Sol gezinti bölmesinde seçin **tüm kaynakları** ve Azure Cosmos DB hesabınızı bulun.  
3. Bir kapsayıcı (koleksiyon, grafik, tablo) veya var olan bir kapsayıcı için güncelleştirme aktarım hızı oluşturulurken, aktarım hızını yapılandırabilirsiniz.  
4. Kapsayıcı oluşturulurken aktarım hızı atamak için açık **Veri Gezgini** dikey penceresinde ve select **yeni koleksiyon** (yeni bir grafik, diğer API'ler için yeni bir tablo)  
5. Formu doldurun **koleksiyon Ekle** dikey penceresi. Bu dikey pencerede alanları aşağıdaki tabloda açıklanmıştır:  

   |**Ayar**  |**Açıklama**  |
   |---------|---------|
   |Veritabanı kimliği  |  Veritabanınızı tanımlamak için benzersiz bir ad belirtin. Veritabanı, bir veya daha fazla koleksiyonların mantıksal bir kapsayıcıdır. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?. |
   |Koleksiyon kimliği  | Koleksiyonunuz tanımlamak için benzersiz bir ad belirtin. Koleksiyon kimliği karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır. |
   |Depolama kapasitesi   | Bu değer, veritabanının depolama kapasitesini temsil eder. Depolama kapasitesi, tek bir koleksiyon için aktarım hızı sağlanırken olabilir **sabit (10 GB)** veya **sınırsız**. Sınırsız depolama kapasitesi, verileriniz için bir bölüm anahtarı ayarlamanızı gerektirir.  |
   |Aktarım hızı   | Her bir koleksiyon ve veritabanı aktarım hızını saniye başına istek birimi olabilir.  Sabit depolama kapasitesi için en düşük aktarım hızı 400 istek birimi (RU/sn) saniyede, için sınırsız depolama kapasitesi, en düşük aktarım hızı 1000 RU/s olarak ayarlanır.|

6. Bu alanlar için değerleri girin, sonra seçin **Tamam** ayarları kaydetmek için.  

   ![Bir koleksiyon için aktarım hızı ayarlama](./media/set-throughput/set-throughput-for-container.png)

7. Var olan bir kapsayıcı için aktarım hızı güncelleştirmek için veritabanı ve kapsayıcısı'nı genişletin ve ardından **ayarları**. Yeni pencerede, yeni aktarım hızı değeri yazın ve ardından **Kaydet**.  

   ![Bir koleksiyon için güncelleştirme aktarım hızı](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Kapsayıcılar veya veritabanı düzeyinde bir kümesi için sağlama aktarım hızı

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Sol gezinti bölmesinde seçin **tüm kaynakları** ve Azure Cosmos DB hesabınızı bulun.  
3. Mevcut bir veritabanı için bir veritabanı veya güncelleştirme aktarım hızı oluşturulurken, aktarım hızını yapılandırabilirsiniz.  
4. Bir veritabanı oluşturulurken aktarım hızı atamak için açık **Veri Gezgini** dikey penceresinde ve select **yeni veritabanı**  
5. Dolgu **veritabanı kimliği** değeri, onay **sağlama aktarım hızı** seçenek ve aktarım hızı değerini yapılandırın.  

   ![Yeni veritabanı seçeneği ile aktarım hızı ayarlama](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Aktarım hızı için varolan bir veritabanını güncelleştirmek için veritabanı ve kapsayıcısı'nı genişletin ve ardından **ölçek**. Yeni pencerede, yeni aktarım hızı değeri yazın ve ardından **Kaydet**.  

   ![Bir veritabanı için güncelleştirme aktarım hızı](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Sağlama aktarım hızı için de bir kapsayıcının bir veritabanında olduğu gibi kapsayıcılar kümesi

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Sol gezinti bölmesinde seçin **tüm kaynakları** ve Azure Cosmos DB hesabınızı bulun.  
3. Veritabanı oluşturma ve aktarım hızı için atayın. Açık **Veri Gezgini** dikey penceresinde ve select **yeni veritabanı**  
4. Dolgu **veritabanı kimliği** değeri, onay **sağlama aktarım hızı** seçenek ve aktarım hızı değerini yapılandırın.  

   ![Yeni veritabanı seçeneği ile aktarım hızı ayarlama](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Sonraki adımda oluşturduğunuz veritabanı içinde bir koleksiyon oluşturun. Bir koleksiyon oluşturmak için veritabanını sağ tıklatın ve **yeni koleksiyon**.  

6. İçinde **koleksiyon Ekle** dikey penceresinde, koleksiyon için bir ad girin ve bölüm anahtarı. İsteğe bağlı olarak, aktarım hızı değerinde atamamayı seçerseniz, bu belirli bir kapsayıcı için aktarım hızı sağlayabilirsiniz, veritabanına atanan aktarım hızı koleksiyona paylaşılır.  

   ![İsteğe bağlı olarak kapsayıcı için aktarım hızı ayarlama](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Aktarım hızı sağlama durumlarda dikkat edilmesi gerekenler

Aşağıda bir aktarım hızı ayırma stratejinize yardımcı olacak bazı önemli noktalar karar verilmiştir.

### <a name="considerations-when-provisioning-throughput-at-the-database-level"></a>Veritabanı düzeyinde aktarım sağlama durumları

Aktarım hızı (yani bir dizi kapsayıcıları için) veritabanı düzeyinde aşağıdaki durumlarda sağlamayı göz önünde bulundurun:

* Aktarım hızı bazı veya tamamı paylaşabildiğini kapsayıcıları bir düzine ya da daha fazla sayıda varsa.  

* Ne zaman Iaas tarafından barındırılan sanal makineleri veya şirket içi (örneğin, NoSQL veya ilişkisel veritabanları için) Azure Cosmos DB'ye çalıştırın ve çok sayıda kapsayıcı olması için tasarlanmış bir tek kiracılı veritabanı geçiriliyor.  

* Havuza alınmış aktarım hızı veritabanı düzeyinde kullanarak iş yüklerini planlanmamış artış dikkate alınması gereken istiyorsanız.  

* Ayar kapsayıcısında aktarım hızını yerine bir tek tek, toplam üretilen iş kapsayıcıları veritabanı içinde bir dizi hale getirmek istiyorsanız.

### <a name="considerations-when-provisioning-throughput-at-the-container-level"></a>Kapsayıcı düzeyinde aktarım sağlama durumları

Aşağıdaki durumlarda bir kapsayıcının aktarım hızı sağlama göz önünde bulundurun:

* Azure Cosmos DB kapsayıcıları daha az sayıda varsa.  

* SLA ile desteklenen belirli bir kapsayıcıda, garantili aktarım hızı almak istiyorsanız.

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>.NET için SQL API'yi kullanarak aktarım hızı ayarlama

### <a name="set-throughput-at-the-container-level"></a>Kapsayıcı düzeyinde aktarım hızı ayarlama
SQL API .NET SDK'sını kullanarak tek bir kapsayıcı için saniyede 3000 istek birimleri ile bir kapsayıcı oluşturmak için kod parçacığı aşağıda verilmiştir:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

### <a name="set-throughput-for-a-set-of-containers-at-the-database-level"></a>Veritabanı düzeyinde bir dizi kapsayıcı için aktarım hızı ayarlama

Kod parçacığı için sağlama 100.000 İşte saniye başına istek birimi bir SQL API .NET SDK kullanarak kapsayıcıları kümesi arasında:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = await client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 });

DocumentCollection sharedCollection1 = new DocumentCollection();
sharedCollection1.Id = "sharedCollection1";
sharedCollection1.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection1, new RequestOptions())

DocumentCollection sharedCollection2 = new DocumentCollection();
sharedCollection2.Id = "sharedCollection2";
sharedCollection2.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, sharedCollection2, new RequestOptions())

DocumentCollection dedicatedCollection = new DocumentCollection();
dedicatedCollection.Id = "dedicatedCollection";
dedicatedCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(database.SelfLink, dedicatedCollection, new RequestOptions { OfferThroughput = 4000 )
```

Azure Cosmos DB, aktarım hızı için bir ayırma modeli üzerinde çalışır. Diğer bir deyişle, aktarım hızı miktarı faturalandırılır *ayrılmış*bakılmaksızın, aktarım hızının ne kadar etkin olduğu *kullanılan*. Uygulamanızı kullanıcının, kolayca ölçeklendirme yapabilen sayısı yukarı ve aşağı yük, verileri ve kullanım desenleri değişiklik ayrılmış RU SDK'ları veya kullanarak [Azure portalı](https://portal.azure.com).

Her kapsayıcı veya kapsayıcılar, bir dizi eşlenmiş bir `Offer` Azure Cosmos DB'de sağlanan aktarım hızı hakkında meta veriler içeren, kaynak. Ayrılmış aktarım hızı, bir kapsayıcı için karşılık gelen teklif kaynak bakarak, daha sonra yeni aktarım hızı değeriyle güncelleştirme değiştirebilirsiniz. .NET SDK kullanarak saniye başına istek birimleri 5.000 bir kapsayıcının aktarım hızını değiştirmek için kod parçacığı aşağıda verilmiştir. Aktarım hızı değiştirdikten sonra var olan Azure portal pencereleri gösterilecek değişen aktarım hızı için yenilemeniz gerekir. 

```csharp
// Fetch the resource to be updated
// For a updating throughput for a set of containers, replace the collection's self link with the database's self link
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Aktarım hızı değiştirdiğinizde kapsayıcınızı kullanılabilirliğini veya kapsayıcılar, bir dizi herhangi bir etkisi yoktur. Genellikle yeni ayrılmış aktarım hızı uygulamasında yeni aktarım hızı, saniye içinde etkili olur.

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Java için SQL API'sini kullanarak aktarım hızını ayarlamak için

Aşağıdaki kod parçacığı, geçerli işleme alır ve 500 RU/s olarak değiştirir. Bir kod örneği için bkz. [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) github'da dosya. 

```Java
// find offer associated with this collection
// To change the throughput for a set of containers, use the database's resource id instead of the collection's resource id
Iterator < Offer > it = client.queryOffers(
    String.format("SELECT * FROM r where r.offerResourceId = '%s'", collectionResourceId), null).getQueryIterator();
assertThat(it.hasNext(), equalTo(true));

Offer offer = it.next();
assertThat(offer.getString("offerResourceId"), equalTo(collectionResourceId));
assertThat(offer.getContent().getInt("offerThroughput"), equalTo(throughput));

// update the offer
int newThroughput = 500;
offer.getContent().put("offerThroughput", newThroughput);
client.replaceOffer(offer);
```

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>MongoDB API portal ölçümleri kullanarak aktarım hızı alma

MongoDB API'si veritabanınızı kullanmak için iyi bir talep tahmin birimi ücreti almak için en basit yolu [Azure portalında](https://portal.azure.com) ölçümleri. İle *istek sayısı* ve *istek yükü* grafikleri, kaç tane istek birimleri her işlemin kullandığı ve kaç tane istek birimleri kullandıkları diğerine göre tahmini elde edebilirsiniz.

![MongoDB API portal ölçümleri][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> MongoDB API'sindeki ayrılmış aktarım hızı sınırlarını aşma
Tüketim oranı sağlanan aktarım hızının altına düşene kadar bir kapsayıcı veya bir dizi kapsayıcı için sağlanan aktarım hızı aşan uygulamaları oranı sınırlı olur. Bir oran sınırlama ortaya çıktığında, arka uç istekle sona erecek bir `16500` hata kodu - `Too Many Requests`. Varsayılan olarak, MongoDB API'sini otomatik olarak en fazla 10 kez döndürmeden önce yeniden deneme bir `Too Many Requests` hata kodu. Çoğu alıyorsanız `Too Many Requests` hata kodları eklemek ya da bir yeniden deneme mantığı, uygulamanızın hata işleme rutinleri düşünmek isteyebilirsiniz veya [kapsayıcı için sağlanan aktarım hızı artırmak](set-throughput.md).

## <a id="GetLastRequestStatistics"></a>MongoDB API GetLastRequestStatistics komutunu kullanarak istek yükü Al

MongoDB API'sini destekleyen özel bir komut *getLastRequestStatistics*, belirtilen işlem için istek ücretleri almak için.

Örneğin, Mongo Kabuğu'nda istek ücreti doğrulamak istediğiniz işlemi yürütün.
```
> db.sample.find()
```

Ardından, komutu yürütün *getLastRequestStatistics*.
```
> db.runCommand({getLastRequestStatistics: 1})
{
    "_t": "GetRequestStatisticsResponse",
    "ok": 1,
    "CommandName": "OP_QUERY",
    "RequestCharge": 2.48,
    "RequestDurationInMilliSeconds" : 4.0048
}
```

Uygulamanız tarafından kullanılan temsili bir ögeye tipik işlemlerin çalıştırmayla ilgili istek birimi ücretine ek olarak kaydedin ve ardından sayısını tahmin etmek için ayrılmış aktarım hızı uygulamanız için gereken miktarı tahmin etmek için bir yöntem olan işlemleri gerçekleştirmek saniyede beklenir.

> [!NOTE]
> Her ilişkilendirilmiş geçerli işlemi istek birimi ücreti, boyutu ve dizini oluşturulmuş özellik sayısı bakımından önemli ölçüde farklılık gösterir öğesi türleriniz varsa, ardından kayıt *türü* tipik öğesi.
> 
> 

## <a name="throughput-faq"></a>Aktarım hızı ile ilgili SSS

**My aktarım hızını 400 RU/s olarak ayarlayabilir miyim?**

400 RU/sn (1000 RU/sn bölümlenmiş kapsayıcılar için en düşük değer) Cosmos DB tek bölüm kapsayıcılarına kullanılabilir en düşük aktarım hızıdır. Birimleri 100 RU/sn aralığı ayarlanmış, ancak aktarım hızı için 100 RU/sn veya herhangi bir değer 400 RU/sn daha küçük olacak şekilde ayarlanamaz isteyin. Geliştirip test Cosmos DB için uygun maliyetli bir yöntem arıyorsanız, ücretsiz kullanabilirsiniz [Azure Cosmos DB öykünücüsü'nü](local-emulator.md), hiçbir ücret ödemeden yerel olarak dağıtabileceğiniz. 

**Aktarım hızı MongoDB API'sini kullanarak nasıl ayarlayabilirim?**

Aktarım hızı ayarlamak için MongoDB API'si uzantısı yok. SQL API'si gösterildiği zamanlayıcısının [.NET için SQL API'sini kullanarak aktarım hızını ayarlamak için](#set-throughput-sdk).

## <a name="next-steps"></a>Sonraki adımlar

* Aktarım hızı ve istek birimlerinin tahmin etme hakkında bilgi edinmek için [istek birimleri ve Azure Cosmos DB'de aktarım tahmin etme](request-units.md)

* Sağlama ve Cosmos DB ile devam eden dünya ölçeğinde hakkında daha fazla bilgi için bkz: [bölümleme ve ölçeklendirme ile Cosmos DB](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
