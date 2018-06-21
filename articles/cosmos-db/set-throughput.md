---
title: Azure Cosmos DB sağlama verimliliğini | Microsoft Docs
description: Azure Cosmos DB containsers, koleksiyonları, grafikler ve tablolar için sağlanan işleme ayarlanacağını öğrenin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/09/2018
ms.author: sngun
ms.openlocfilehash: d8b7ed593fcd307e6709c17bafbcb5a22661dc83
ms.sourcegitcommit: d8ffb4a8cef3c6df8ab049a4540fc5e0fa7476ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2018
ms.locfileid: "36285782"
---
# <a name="set-and-get-throughput-for-azure-cosmos-db-containers-and-database"></a>Ayarlama ve Azure Cosmos DB kapsayıcıları ve veritabanı için işleme alma

Azure portalını kullanarak veya istemci SDK'ları kullanarak, bir Azure Cosmos DB kapsayıcısı veya bir dizi kapsayıcıları için işleme ayarlayabilirsiniz. Üretilen iş kapsayıcıları kümesi için hazırlarken bu tüm kapsayıcıları sağlanan işleme paylaşır. Tek tek kapsayıcıları için sağlama işleme için belirli bir kapsayıcıya işleme ayırma garanti. Öte yandan, bir veritabanı için işleme sağlama veritabanına ait tüm kapsayıcıları arasında verimliliği paylaşmanıza olanak tanır. Bir Azure Cosmos DB veritabanı içinde işleme ayrılmış kapsayıcıları yanı sıra işleme paylaşan kapsayıcıları kümesi olabilir. 

Üzerinde sağlanan işleme bağlı olarak, Azure Cosmos DB onu büyüdükçe kaldırıldığından ve bölmelerini/rebalances verilerinizi bölümler barındırmak için fiziksel bölümleri ayırır.

Olarak RU/sn tek tek kapsayıcı düzeyinde atarken kapsayıcıları oluşturulabilir *sabit* veya *sınırsız*. Sabit boyutlu kapsayıcıların üst sınırı 10 GB ve 10.000 RU/sn aktarım hızıdır. Sınırsız bir kapsayıcı oluşturmak için en düşük işleme 1.000 RU/s belirtmeniz gerekir ve bir [bölüm anahtarı](partition-data.md). Verilerinizin birden çok bölüm arasında bölünmesi gerekebilir olduğundan, yüksek kardinalite (100 farklı değerleri milyonlarca) sahip bir bölüm anahtarı almak gereklidir. Birçok farklı değerlere sahip bir bölüm anahtarı seçerek tablo/kapsayıcı/grafik ve istekleri hep Azure Cosmos DB tarafından Genişletilebilir emin olun. 

Bu kümeye ait kapsayıcıları RU/sn kapsayıcıları kümesi boyunca atarken davranılır *sınırsız* kapsayıcıları ve bölüm anahtarı belirtmeniz gerekir.

![İstek birimleri ayrı kapsayıcıları ve kapsayıcıları kümesi için sağlama](./media/request-units/provisioning_set_containers.png)

Bu makalede, Azure Cosmos DB hesap için farklı düzeylerde işleme yapılandırmak için gerekli adımları açıklanmaktadır. 

## <a name="provision-throughput-by-using-azure-portal"></a>Azure portalını kullanarak hazırlama işleme

### <a name="provision-throughput-for-a-container-collectiongraphtable"></a>Bir kapsayıcı (grafik/koleksiyonu/tablosu) için sağlama işleme

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Sol gezinti seçin **tüm kaynakları** ve Azure Cosmos DB hesabınızı bulabilirsiniz.  
3. Bir kapsayıcı (koleksiyon, grafik, tablo) veya var olan bir kapsayıcı için güncelleştirme işleme oluşturulurken verimlilik yapılandırabilirsiniz.  
4. Bir kapsayıcı oluşturulurken verimlilik atamak için açın **Veri Gezgini** dikey penceresinde ve select **yeni koleksiyon** (yeni bir grafik, diğer API'leri için yeni bir tablo)  
5. Formu doldurun **topluluk Ekle** dikey. Alanlar bu dikey penceresinde aşağıdaki tabloda açıklanmıştır:  

   |**Ayar**  |**Açıklama**  |
   |---------|---------|
   |Veritabanı kimliği  |  Veritabanınızı tanımlamak için benzersiz bir ad sağlayın. Veritabanı, bir veya daha fazla koleksiyonların mantıksal bir kapsayıcısıdır. Veritabanı adı 1 ila 255 karakterden oluşmalı, boşlukla bitmemeli ve şu karakterleri içermemelidir: /, \\, # ve ?. |
   |Koleksiyon kimliği  | Koleksiyonunuz tanımlamak için benzersiz bir ad sağlayın. Koleksiyon kimliği karakter gereksinimleri, veritabanı adlarına ilişkin karakter gereksinimleri ile aynıdır. |
   |Depolama kapasitesi   | Bu değer, veritabanı depolama kapasitesini temsil eder. Tek bir koleksiyon için işleme sağlanırken, depolama kapasitesi olabilir **sabit (10 GB)** veya **sınırsız**. Sınırsız depolama kapasitesi, verileriniz için bir bölüm anahtarı ayarlamanızı gerektirir.  |
   |Aktarım hızı   | Her bir koleksiyon ve veritabanı işleme saniye başına istek birimleri olabilir.  Sabit depolama kapasitesi en düşük işleme 400 istek birimleri (RU/s) saniye başına, sınırsız depolama için kapasite ve minimum verimlilik 1000 RU/s için ayarlanır.|

6. Bu alanlar için değerleri girdikten sonra Seç **Tamam** ayarları kaydetmek için.  

   ![Bir koleksiyon için kümesi işleme](./media/set-throughput/set-throughput-for-container.png)

7. Var olan bir kapsayıcı için işleme güncelleştirmek için veritabanı ve kapsayıcısını genişletin ve ardından **ayarları**. Yeni pencerede yeni işleme değeri yazın ve ardından **kaydetmek**.  

   ![Bir koleksiyon için güncelleştirme işleme](./media/set-throughput/update-throughput-for-container.png)

### <a name="provision-throughput-for-a-set-of-containers-or-at-the-database-level"></a>Kapsayıcıların veya veritabanı düzeyinde kümesi için sağlama işleme

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Sol gezinti seçin **tüm kaynakları** ve Azure Cosmos DB hesabınızı bulabilirsiniz.  
3. Var olan bir veritabanı için bir veritabanı veya güncelleştirme işleme oluşturulurken verimlilik yapılandırabilirsiniz.  
4. Bir veritabanı oluşturulurken verimlilik atamak için açık **Veri Gezgini** dikey penceresinde ve select **yeni veritabanı**  
5. Dolgu **veritabanı kimliği** değeri, onay **sağlama verimlilik** seçeneği ve üretilen iş değerini yapılandırın. Bir veritabanı en düşük işleme değerle 50.000 sağlanabilir RU/s.  

   ![İşleme yeni veritabanı seçeneği ile ayarlama](./media/set-throughput/set-throughput-with-new-database-option.png)

6. Üretilen iş için varolan bir veritabanını güncelleştirmek için veritabanı ve kapsayıcısını genişletin ve ardından **ölçek**. Yeni pencerede yeni işleme değeri yazın ve ardından **kaydetmek**.  

   ![Bir veritabanı için güncelleştirme işleme](./media/set-throughput/update-throughput-for-database.png)

### <a name="provision-throughput-for-a-set-of-containers-as-well-as-for-an-individual-container-in-a-database"></a>Bir veritabanında tek tek bir kapsayıcı için de kapsayıcıları kümesi için sağlama işleme

1. [Azure Portal](https://portal.azure.com) oturum açın.  
2. Sol gezinti seçin **tüm kaynakları** ve Azure Cosmos DB hesabınızı bulabilirsiniz.  
3. Bir veritabanı oluşturun ve verimlilik atayın. Açık **Veri Gezgini** dikey penceresinde ve select **yeni veritabanı**  
4. Dolgu **veritabanı kimliği** değeri, onay **sağlama verimlilik** seçeneği ve üretilen iş değerini yapılandırın. Bir veritabanı en düşük işleme değerle 50.000 sağlanabilir RU/s.  

   ![İşleme yeni veritabanı seçeneği ile ayarlama](./media/set-throughput/set-throughput-with-new-database-option.png)

5. Ardından, yukarıdaki adım oluşturduğunuz veritabanı içinde bir koleksiyon oluşturun. Bir koleksiyon oluşturmak için veritabanını sağ tıklatın ve **yeni koleksiyon**.  

6. İçinde **topluluk Ekle** dikey penceresinde, koleksiyon için bir ad girin ve bölüm anahtarı. İsteğe bağlı olarak, bir işleme değer atamama seçerseniz, belirli bir kapsayıcıya için işleme sağlayabilir, veritabanına atanan verimlilik koleksiyona paylaşılan.  

   ![İsteğe bağlı olarak işleme için kapsayıcı ayarlama](./media/set-throughput/optionally-set-throughput-for-the-container.png)

## <a name="considerations-when-provisioning-throughput"></a>Üretilen iş sağlama ilgili önemli noktalar

Bir işleme ayırma stratejinize yardımcı bazı noktalar karar aşağıda verilmiştir.

Üretilen iş düzeyinde (yani kapsayıcıları kümesi için) veritabanını aşağıdaki durumlarda sağlama göz önünde bulundurun:

* Üretilen iş bazılarını veya tümünü arasında paylaşabilir kapsayıcıları bir düzine veya daha fazla sayıda varsa.  

* Iaas barındırılan sanal makineleri veya şirket içi (örneğin, NoSQL veya ilişkisel veritabanları) için Azure Cosmos DB çalıştırmak ve birçok kapsayıcıları için tasarlanmış bir tek Kiracı veritabanından ne zaman geçirdiğiniz.  

* Veritabanı düzeyinde havuza alınmış verimlilik kullanarak iş yüklerinde planlanmamış ani göz önünde bulundurun istiyorsanız.  

* Tek bir kapsayıcısı üzerinde ayarı verimlilik yerine kümesi veritabanı kapsayıcılara boyunca toplam verimlilik hale getirmek.

Aşağıdaki durumlarda tek tek bir kapsayıcı, üretilen iş sağlama göz önünde bulundurun:

* Daha az sayıda Azure Cosmos DB kapsayıcıları varsa.  

* Garantili verimlilik destekli bir SLA tarafından verilen bir kapsayıcı almak istiyorsanız.

## <a name="throughput-ranges"></a>Üretilen iş aralıkları

Aşağıdaki tabloda, üretilen iş için kapsayıcıları kullanılabilir listelenmektedir:

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p></p></td>
            <td valign="top"><p><strong>Tek bir bölüm kapsayıcısı</strong></p></td>
            <td valign="top"><p><strong>Bölümlenmiş kapsayıcısı</strong></p></td>
            <td valign="top"><p><strong>Kapsayıcıları kümesi</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>En düşük işleme</p></td>
            <td valign="top"><p>saniye başına 400 istek birimleri</p></td>
            <td valign="top"><p>saniye başına 1000 istek birimleri</p></td>
            <td valign="top"><p>saniye başına 50.000 istek birimleri</p></td>
        </tr>
        <tr>
            <td valign="top"><p>En yüksek verimlilik</p></td>
            <td valign="top"><p>saniye başına 10.000 istek birimleri</p></td>
            <td valign="top"><p>Sınırsız</p></td>
            <td valign="top"><p>Sınırsız</p></td>
        </tr>
    </tbody>
</table>

<a id="set-throughput-sdk"></a>

## <a name="set-throughput-by-using-sql-api-for-net"></a>.NET için SQL API'yi kullanarak kümesi işleme

SQL API'nin .NET SDK kullanarak tek bir kapsayıcı için saniye başına 3000 istek birimleri ile bir kapsayıcı oluşturmak için bir kod parçacığı aşağıda verilmiştir:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Sağlama 100.000 için bir kod parçacığı aşağıda verilmiştir saniye başına birim kapsayıcıları SQL API'nin .NET SDK kullanarak bir dizi arasında iste:

```csharp
// Provision 100,000 RU/sec at the database level. 
// sharedCollection1 and sharedCollection2 will share the 100,000 RU/sec from the parent database
// dedicatedCollection will have its own dedicated 4,000 RU/sec, independant of the 100,000 RU/sec provisioned from the parent database
Database database = client.CreateDatabaseAsync(new Database { Id = "myDb" }, new RequestOptions { OfferThroughput = 100000 }).Result;

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

Üretilen iş için bir ayırma modeli Azure Cosmos DB çalıştırır. Diğer bir deyişle, işleme miktarı faturalandırılır *ayrılmış*ne olursa olsun, üretilen işi ne kadarının etkin olduğundan, *kullanılan*. Uygulamanızı olarak kullanıcının, kolayca ölçeklendirilebilir sayısı yukarı ve aşağı yük, veri ve kullanım desenlerini değişiklik ayrılmış RUs üzerinden SDK'ları veya kullanarak [Azure Portal](https://portal.azure.com).

Her kapsayıcı veya kapsayıcıları, kümesi eşlenmiş bir `Offer` kaynak, sağlanan işleme hakkındaki meta verileri olan Azure Cosmos veritabanı. Kapsayıcı için ilgili teklif kaynak bakarak, ardından yeni işleme değeri ile güncelleştirme ayrılmış işleme değiştirebilirsiniz. .NET SDK kullanarak saniye başına 5.000 istek birimi için bir kapsayıcı verimini değiştirmek için bir kod parçacığı aşağıda verilmiştir. Üretilen iş değiştirdikten sonra var olan Azure portal pencereleri gösterilmeye değiştirilen üretilen iş için yenilemeniz gerekir. 

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

Üretilen iş değiştirdiğinizde, kapsayıcı kullanılabilirliğini veya kapsayıcıları, kümesi üzerinde etkisi yoktur. Genellikle yeni ayrılmış işleme yeni işleme, uygulama üzerinde saniye içinde etkili olur.

<a id="set-throughput-java"></a>

## <a name="to-set-the-throughput-by-using-the-sql-api-for-java"></a>Java için SQL API'yi kullanarak tarafından üretilen işi ayarlamak için

Aşağıdaki kod parçacığını geçerli işleme alır ve 500 RU/s değiştirir. Tam bir kod örneği için bkz: [OfferCrudSamples.java](https://github.com/Azure/azure-documentdb-java/blob/master/documentdb-examples/src/test/java/com/microsoft/azure/documentdb/examples/OfferCrudSamples.java) github'da dosya. 

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

## <a id="GetLastRequestStatistics"></a>MongoDB API'nin GetLastRequestStatistics komutunu kullanarak işleme al

Özel bir komut MongoDB API destekler *getLastRequestStatistics*, belirtilen işlem için istek ücret alma.

Örneğin, Mongo kabuğunu isteği ücreti doğrulamak istediğiniz işlemi yürütün.
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

Uygulamanızın gerektirdiği ayrılmış işleme miktarı tahmin etmek için bir yöntem olduğu uygulamanız tarafından kullanılan bir temsili öğesi karşı normal işlemleri çalıştırılması ile ilişkili istek birimi ücret kaydetmek ve sayısını tahmin etme Operations saniyede gerçekleştirmek beklenir.

> [!NOTE]
> Hangi boyutu ve Dizinli Özellikler sayısı bakımından önemli ölçüde farklılık gösterecektir öğesi türleriniz varsa, her ilişkilendirilmiş geçerli işlem istek birimi ücret kayıt *türü* tipik öğesi.
> 
> 

## <a name="get-throughput-by-using-mongodb-api-portal-metrics"></a>MongoDB API portal ölçümleri kullanarak işleme alma

En basit yolu MongoDB API veritabanınızı kullanmak için bir iyi isteğinin birim ücretleri kestirmek [Azure portal](https://portal.azure.com) ölçümleri. İle *istek sayısı* ve *isteği ücret* grafikler, kaç tane istek birimleri her işlem harcayan ve kaç tane istek birimleri bunlar tüketen birbirine göre tahmini alabilirsiniz.

![MongoDB API portal ölçümleri][1]

### <a id="RequestRateTooLargeAPIforMongoDB"></a> MongoDB API aşan ayrılmış işleme sınırları
Tüketim oranını sağlanan işleme hızının altına düşene kadar bir kapsayıcı veya kapsayıcıları kümesi için sağlanan işleme aşan uygulamalar oranı sınırlı olur. Bir hızını sınırlama ortaya çıktığında, arka uç istekle sona erer bir `16500` hata kodu - `Too Many Requests`. Varsayılan olarak, MongoDB API otomatik olarak en fazla 10 kez döndürmeden önce yeniden deneme bir `Too Many Requests` hata kodu. Birçok alıyorsanız `Too Many Requests` hata kodları, ya da yeniden deneme mantığı, uygulamanızın hata yordamları işleme ekleme düşünmek isteyebilirsiniz veya [kapsayıcı sağlanan verimliliğini artırmak](set-throughput.md).

## <a name="throughput-faq"></a>Üretilen iş ile ilgili SSS

**My verimlilik değerinden 400 RU/s ayarlayabilir miyim?**

400 RU/s (1000 RU/s bölümlenmiş kapsayıcıları için en düşük gereksinimdir) Cosmos DB tek bölüm kapsayıcılarında kullanılabilir en düşük işleme ' dir. İstek birimleri 100 RU/s aralıklarla ayarlanmış, ancak işleme 100 RU/s veya herhangi bir değer 400 RU/s küçük olacak şekilde ayarlanamaz. Geliştirmek ve Cosmos DB sınamak için uygun maliyetli bir yöntem arıyorsanız, ücretsiz kullanabileceğiniz [Azure Cosmos DB öykünücüsü](local-emulator.md), hangi hiçbir ücret yerel olarak dağıtabilirsiniz. 

**MongoDB API kullanarak verimlilik nasıl ayarlarım?**

Üretilen iş ayarlamak için MongoDB API uzantısı yok. SQL API kullanan gösterildiği gibi önerilir [.NET için SQL API'yi kullanarak üretilen işi ayarlamak için](#set-throughput-sdk).

## <a name="next-steps"></a>Sonraki adımlar

* Üretilen iş ve istek birimleri tahmin etme hakkında bilgi edinmek için [istek birimleri & Azure Cosmos DB performansı tahmin etme](request-units.md)

* Sağlama ve devam eden planet ölçekli Cosmos DB ile hakkında daha fazla bilgi için bkz: [bölümleme ve Cosmos DB ile ölçeklendirme](partition-data.md).

[1]: ./media/set-throughput/api-for-mongodb-metrics.png
