---
title: Birimleri iste & verimlilik - Azure Cosmos DB tahmin etme | Microsoft Docs
description: Anlamak, belirtin ve Azure Cosmos veritabanı istek birimi gereksinimlerini tahmin etme hakkında bilgi edinin.
services: cosmos-db
author: mimig1
manager: jhubbard
editor: mimig
documentationcenter: ''
ms.assetid: d0a3c310-eb63-4e45-8122-b7724095c32f
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/30/2018
ms.author: mimig
ms.openlocfilehash: 5f733e9cbd90829eded80b1401093e2331a1eb16
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2018
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB birimlerinde isteği

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Microsoft'un Genel dağıtılmış birden çok model veritabanıdır. Azure Cosmos DB ile sanal makineleri kiralamak, yazılım dağıtma veya veritabanlarını izleme gerekmez. Azure Cosmos DB işletilen ve sürekli olarak world sınıfı kullanılabilirliği, performansı ve veri koruma sağlamak üzere Microsoft üst mühendisleri tarafından izlenir. Tercih ettiğiniz API'leri gibi kullanarak, verilerinizi erişebilirsiniz [SQL API](documentdb-introduction.md), [MongoDB API](mongodb-introduction.md), [tablo API](table-introduction.md)ve Gremlin aracılığıyla [grafik API'si](graph-introduction.md) - tüm yerel olarak desteklenir. Azure Cosmos DB para istek birimi (RU) birimidir. RUs ile okuma/yazma kapasiteleri veya sağlama CPU, bellek ve IOPS ayırmak gerekmez.

Azure Cosmos DB Basit okuma arasında değişen farklı işlemlerle API'lerini destekler ve karmaşık grafik sorguları yazar. Tüm istekleri eşit olduğundan, normalleştirilmiş bir miktar atanan **istek birimleri** isteğe hizmet vermek için gerekli hesaplama miktarına göre. Bir işlem için istek birim sayısı belirleyici ve bir yanıt üstbilgisi aracılığıyla Azure Cosmos veritabanı herhangi bir işlem tarafından kullanılan istek birim sayısını izleyebilirsiniz. 

Tahmin edilebilir performans sağlamak için işleme birimi 100 RU/saniye yedek gerekir. Yapabilecekleriniz [, üretilen iş gereken tahmin](request-units.md#estimating-throughput-needs) Azure Cosmos DB kullanarak [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).

![Üretilen iş hesaplayıcısı][5]

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:  

* Hangi birimleri istemek ve ücretleri isteği?
* Bir kapsayıcı için istek birim kapasitesi nasıl belirteceğinizi?
* My uygulamanın istek birimi gereken nasıl tahmin?
* Bir kapsayıcı için istek birim kapasitesi aşmanız durumunda ne olur?

Azure Cosmos DB çok model veritabanı olduğundan, bu makale için bir belge API, grafik API'si için bir grafik/düğümü ve tablo API için bir tablo/varlık koleksiyonu/belgeye başvuruyor dikkate almak önemlidir. Bu makale koleksiyonu, grafik veya tablo bir kapsayıcı olarak kavramı başvuruyor ve bir belge, düğüm veya varlık öğe olarak.

## <a name="request-units-and-request-charges"></a>İstek birimleri ve istek ücretleri
Azure Cosmos DB tarafından hızlı ve tahmin edilebilir performans sunar *ayırma* uygulamanızın verimlilik gereken karşılamak için kaynakları.  Uygulama yüklemek ve zaman içinde desenleri değişiklik erişmek için Azure Cosmos DB kolayca artırın veya uygulamanız için kullanılabilir ayrılmış işleme miktarını azaltmak sağlar.

Azure Cosmos DB ile ayrılmış işleme saniyede işlediği istek birimler cinsinden belirtilir. İstek birimleri yapabildiği verimlilik para birimi olarak düşünebilirsiniz, *yedek* garantili istek birimleri uygulamanıza kullanılabilir miktardaki saniye başına temelinde.  Azure Cosmos - bir belge yazma, bir belge güncelleştirme bir sorgu gerçekleştirme - DB her bir işlemin CPU, bellek ve IOPS tüketir.  Diğer bir deyişle, her işlemi uygulanan bir *isteği ücret*, içinde ifade *istek birimleri*.  İstek birimi ücretleri, uygulamanızın işleme gereksinimleri etkileyen faktörler anlama, uygulamanızın maliyeti etkili bir şekilde olabildiğince olarak çalıştırmak sağlar. Azure portalında Veri Gezgini ayrıca bir sorgu çekirdek test etmek için bir harika aracıdır.

Burada Azure Cosmos DB Program Yöneticisi Barış Liu istek birimleri anlatılmaktadır aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz.

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="specifying-request-unit-capacity-in-azure-cosmos-db"></a>İstek birimi kapasite Azure Cosmos DB'de belirtme
Yeni bir kapsayıcı başlatırken numarası belirtin (RU saniye başına) saniyede istek birimlerinin ayrılmış istiyor. Üzerinde sağlanan işleme bağlı olarak, Azure Cosmos DB, kapsayıcı barındırmak için fiziksel bölümleri ayırır ve bölmelerini/veri bölümler, büyüdükçe rebalances.

Azure Cosmos DB kapsayıcıları sabit olarak oluşturulabilir veya sınırsız. Sabit boyutlu kapsayıcıların üst sınırı 10 GB ve 10.000 RU/sn aktarım hızıdır. Sınırsız bir kapsayıcı oluşturmak için en düşük işleme 1.000 RU/s belirtin ve bir [bölüm anahtarı](partition-data.md). Verilerinizin birden çok bölüm arasında bölünmesi gerekebilir olduğundan, yüksek kardinalite (100 farklı değerleri milyonlarca) sahip bir bölüm anahtarı almak gereklidir. Birçok farklı değerlere sahip bir bölüm anahtarı seçerek tablo/kapsayıcı/grafik ve istekleri hep Azure Cosmos DB tarafından Genişletilebilir emin olun. 

> [!NOTE]
> Bölüm anahtarı mantıksal bir sınır ve fiziksel bir tane ' dir. Bu nedenle, farklı bölüm anahtar değerlerin sayısını sınırlamak gerekmez. Aslında, daha fazla yük dengeleme seçeneklerini Azure Cosmos DB sahip daha farklı bölüm anahtarı değerden daha az olması iyidir.

.NET SDK kullanarak saniye başına istek birimleri 3000 ile bir kapsayıcı oluşturmak için bir kod parçacığı aşağıda verilmiştir:

```csharp
DocumentCollection myCollection = new DocumentCollection();
myCollection.Id = "coll";
myCollection.PartitionKey.Paths.Add("/deviceId");

await client.CreateDocumentCollectionAsync(
    UriFactory.CreateDatabaseUri("db"),
    myCollection,
    new RequestOptions { OfferThroughput = 3000 });
```

Azure Cosmos DB akışındaki bir ayırma modeli çalıştırır. Diğer bir deyişle, işleme miktarı faturalandırılır *ayrılmış*ne olursa olsun, üretilen işi ne kadarının etkin olduğundan, *kullanılan*. Uygulamanızı olarak kullanıcının, kolayca ölçeklendirilebilir miktarını yukarı ve aşağı yük, veri ve kullanım desenlerini değişiklik ayrılmış RUs üzerinden SDK'ları veya kullanarak [Azure Portal](https://portal.azure.com).

Her kapsayıcı eşlenmiş bir `Offer` kaynak, sağlanan işleme hakkındaki meta verileri olan Azure Cosmos veritabanı. Kapsayıcı için ilgili teklif kaynak bakarak, ardından yeni işleme değeri ile güncelleştirme ayrılmış işleme değiştirebilirsiniz. .NET SDK kullanarak saniye başına 5.000 istek birimi için bir kapsayıcı verimini değiştirmek için bir kod parçacığı aşağıda verilmiştir:

```csharp
// Fetch the resource to be updated
Offer offer = client.CreateOfferQuery()
                .Where(r => r.ResourceLink == collection.SelfLink)    
                .AsEnumerable()
                .SingleOrDefault();

// Set the throughput to 5000 request units per second
offer = new OfferV2(offer, 5000);

// Now persist these changes to the database by replacing the original resource
await client.ReplaceOfferAsync(offer);
```

Üretilen iş değiştirdiğinizde, kapsayıcı kullanılabilirliğine hiçbir etkisi yoktur. Genellikle yeni ayrılmış işleme yeni işleme, uygulama üzerinde saniye içinde etkili olur.

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Genel olarak dağıtılmış veritabanlarında işleme yalıtımı

Birden fazla bölgeye veritabanınızı çoğaltıldığından, Azure Cosmos DB RU kullanım bir bölgede başka bir bölgede RU kullanım etkilememesini sağlamak için işleme yalıtım sağlar. Örneğin, tek bir bölge için veri yazma ve başka bir bölgesinden veri okuma, B. RUs dağıttıktan sonra bölgeler arasında bölünür değil bölgede okuma işlemi için kullanılan RUs çıktığınızda bölge A yazma işlemi gerçekleştirmek için kullanılan RUs kazanmaz. Veritabanı çoğaltılan her bölge sağlanan RUs tam miktarına sahip. Genel çoğaltma hakkında daha fazla bilgi için bkz: [Azure Cosmos DB genel verilerle dağıtmak nasıl](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>İstek birimi konuları
Azure Cosmos DB kapsayıcısı için ayrılacak isteği birim sayısını tahmin yaparken, aşağıdaki değişkenleri dikkate önemlidir:

* **Öğe boyutunu**. Boyutu okumak veya veri da artırır yazmak için kullanılan birimleri arttıkça.
* **Madde özellik sayısı**. Tüm özelliklerin, bir belge/varlık başına düğüm artış özellik sayısı arttıkça yazmak için kullanılan birimleri varsayılan dizin varsayılır.
* **Veri tutarlılığı**. Veri tutarlılık düzeylerini güçlü veya sınırlanmış eskime durumu kullanırken, ek birimler öğeleri okumak için kullanılır.
* **Dizin oluşturulmuş özellikleri**. Bir dizin İlkesi her kapsayıcısı üzerinde varsayılan olarak hangi özellikleri dizinlenir belirler. Dizinli Özellikler sayısını sınırlayarak veya yavaş dizin etkinleştirerek, istek birimi tüketimini azaltabilirsiniz.
* **Belge dizine**. Varsayılan olarak, her bir öğeyi otomatik olarak dizine alınır. Bazı öğelerinizi dizin kullanılamıyor seçerseniz daha az istek birimi kullanabilir.
* **Sorgu desenleri**. Kaç tane istek birimlerine bir işlem için kullanılan bir sorgu karmaşıklığını etkiler. Koşulları sayısı, koşulları, projeksiyonları, UDF'ler sayısı ve tüm kaynak veri kümesi boyutunu yapısını sorgu işlemlerinin maliyetini etkiler.
* **Komut dosyası kullanımı**.  Sorguları olarak gerçekleştirilen işlemler kapsamına bağlı istek birimleri saklı yordamları ve Tetikleyicileri tüketir. Uygulamanızı geliştirirken, her işlem isteği birim kapasitesi nasıl tüketen daha iyi anlamak için istek ücret üstbilgisi inceleyin.

## <a name="estimating-throughput-needs"></a>Üretilen iş gereksinimlerini tahmin etme
Bir istek birimi istek maliyet işleme normalleştirilmiş ölçüsüdür. Bir tek istek birimi (kendi bağlantısını veya kimliği) öğesi 10 benzersiz özellik değerlerini (Sistem özellikleri dışında) oluşan tek 1 bb okumak için gereken işlem kapasitesi temsil eder. Daha fazla hizmet ve böylece işleme daha fazla istek birimi (Ekle) oluşturmak, değiştirmek veya aynı öğeyi silmek için bir istek tüketir.   

> [!NOTE]
> 1 istek birimi bir 1 KB öğesi için taban çizgisi için basit bir GET kendi bağlantısını veya öğenin kimliği tarafından karşılık gelir.
> 
> 

Örneğin, üç farklı öğe boyut (1 KB, 4 KB ve 64 KB) ve iki farklı performans düzeyleri sağlamak için kaç tane istek birimleri gösteren bir tablo İşte (500 Okuma/saniye 100 yazma/saniye ve 500 Okuma/saniye + 500 yazma/saniye). Veri tutarlılığı oturumunda yapılandırılmış ve dizin oluşturma ilkesi None olarak ayarlanmış.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Öğesi boyutu</strong></p></td>
            <td valign="top"><p><strong>Reads/second</strong></p></td>
            <td valign="top"><p><strong>Writes/second</strong></p></td>
            <td valign="top"><p><strong>İstek birimleri</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1,000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3,000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1.3) + (100 * 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1.3) + (500 * 7) = 4,150 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 10) + (100 * 48) = 9,800 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>64 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 10) + (500 * 48) = 29,000 RU/s</p></td>
        </tr>
    </tbody>
</table>

### <a name="use-the-request-unit-calculator"></a>İstek birimi hesaplayıcı kullanın
Kendi işleme tahminler ince ayar müşterilere yardımcı olmak için olduğundan web tabanlı [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner) genel işlemler de dahil olmak üzere, istek birimi gereksinimlerini tahmin etmeye yardımcı olması için:

* (Yazar) öğesi oluşturur
* Öğe okur
* Öğeyi siler
* Öğesi güncelleştirmeleri

Aracı, sağladığınız örnek öğeleri temel alan veri depolama gereksinimlerini tahmin etmek için destek de içerir.

Aracını kullanarak basit bir işlemdir:

1. Bir veya daha fazla temsilcisi öğeleri karşıya yükleyin.
   
    ![İstek birimi makinesine öğeleri karşıya yükle][2]
2. Veri depolama gereksinimlerini tahmin etmek için toplam depolamayı beklediğiniz öğe sayısını girin.
3. Girin öğe sayısı oluşturma, okuma, güncelleştirme ve silme işlemleri (bir saniye başına temelinde) gerektirir. İstek birimi ücretleri öğesi güncelleştirme işlemlerinin tahmin etmek için 1. adımdaki tipik alan güncelleştirmeleri içeren Yukarıdaki örnek öğenin bir kopyasını yükleyin.  Örneğin, öğesi güncelleştirmeler genellikle lastLogin ve userVisits adlı iki özellik değiştirirseniz, ardından yalnızca örnek öğesi kopyalayın, bu iki özellik için değerleri güncelleştirmek ve kopyalanan öğeyi karşıya yükleyin.
   
    ![Üretilen iş gereksinimleri istek birimi hesaplayıcı girin][3]
4. Tıklatın hesaplamak ve sonuçları inceleyin.
   
    ![Birim hesaplayıcı sonuçları isteği][4]

> [!NOTE]
> Boyutu ve Dizinli Özellikler sayısı bakımından önemli ölçüde farklılık gösterecektir öğesi türleri varsa, her bir örnek karşıya yükleme *türü* tipik öğesi için araç ve sonuçları hesaplayın.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Azure Cosmos DB istek ücret yanıt üstbilgisi kullanın
Her yanıt Azure Cosmos DB hizmetinden bir özel üst bilgi içeriyor (`x-ms-request-charge`) istek için kullanılan istek birimleri içerir. Bu üst ayrıca Azure Cosmos DB SDK erişilebilir. .NET SDK'ın RequestCharge ResourceResponse nesnesinin bir özelliğidir.  Sorgular için Azure portalında Azure Cosmos DB Veri Gezgini yürütülen sorgular için istek ücret bilgileri sağlar.

Bu durum dikkate alınarak, uygulamanızın gerektirdiği ayrılmış işleme miktarı tahmin etmek için bir yöntem, uygulamanız tarafından kullanılan ve ardından tahmin etme temsili bir öğe karşı çalışan tipik işlemlerle ilişkili istek birimi ücret kaydetmektir saniyede gerçekleştirme düşündüğünüz işlemlerinin sayısı.  Ölçmek ve tipik sorgular ve Azure Cosmos DB komut dosyası kullanımı da dahil emin olun.

> [!NOTE]
> Önemli ölçüde boyutu ve Dizinli Özellikler sayısı bakımından farklılık gösterir öğesi türleriniz varsa, her ilişkilendirilmiş geçerli işlem istek birimi ücret kayıt *türü* tipik öğesi.
> 
> 

Örneğin:

1. (Ekleme) oluşturma isteği birim ücret kayıt tipik bir öğe. 
2. Tipik bir öğe Okuma isteği birim ücret kaydedin.
3. Tipik bir öğesi güncelleştirme isteği birim ücret kaydedin.
4. Tipik, ortak öğesi sorgularını istek birimi olarak kaydedin.
5. Uygulama tarafından işlevden istek birimi ücret tüm özel komut dosyalarını (saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler), kayıt
6. Saniyede çalıştırmayı düşündüğünüz operations tahmini sayısını verilen gerekli istek birimleri hesaplayın.

## <a id="GetLastRequestStatistics"></a>API MongoDB'ın GetLastRequestStatistics komutu için kullanın
Özel bir komut MongoDB API destekler *getLastRequestStatistics*, belirtilen işlem için istek ücret alma.

Örneğin, istek ücreti doğrulamak istediğiniz işlem Mongo kabuğunu yürütün.
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

Bu durum dikkate alınarak, uygulamanızın gerektirdiği ayrılmış işleme miktarı tahmin etmek için bir yöntem, uygulamanız tarafından kullanılan ve ardından tahmin etme temsili bir öğe karşı çalışan tipik işlemlerle ilişkili istek birimi ücret kaydetmektir saniyede gerçekleştirme düşündüğünüz işlemlerinin sayısı.

> [!NOTE]
> Hangi boyutu ve Dizinli Özellikler sayısı bakımından önemli ölçüde farklılık gösterecektir öğesi türleriniz varsa, her ilişkilendirilmiş geçerli işlem istek birimi ücret kayıt *türü* tipik öğesi.
> 
> 

## <a name="use-mongodb-api-portal-metrics"></a>MongoDB API portal ölçümleri kullanın
MongoDB API veritabanınızı kullanmak için iyi bir tahmin isteğinin birim ücretleri almak için en basit yolu [Azure portal](https://portal.azure.com) ölçümleri. İle *istek sayısı* ve *isteği ücret* grafikler, kaç tane isteği birimlerin her işlem harcayan ve kaç tane istek birimleri bunlar tüketen birbirine göre bir tahmin alabilirsiniz.

![MongoDB API portal ölçümleri][6]

## <a name="a-request-unit-estimation-example"></a>Bir istek birimi tahmin örneği
Aşağıdaki ~ 1 KB belge göz önünde bulundurun:

```json
{
 "id": "08259",
  "description": "Cereals ready-to-eat, KELLOGG, KELLOGG'S CRISPIX",
  "tags": [
    {
      "name": "cereals ready-to-eat"
    },
    {
      "name": "kellogg"
    },
    {
      "name": "kellogg's crispix"
    }
  ],
  "version": 1,
  "commonName": "Includes USDA Commodity B855",
  "manufacturerName": "Kellogg, Co.",
  "isFromSurvey": false,
  "foodGroup": "Breakfast Cereals",
  "nutrients": [
    {
      "id": "262",
      "description": "Caffeine",
      "nutritionValue": 0,
      "units": "mg"
    },
    {
      "id": "307",
      "description": "Sodium, Na",
      "nutritionValue": 611,
      "units": "mg"
    },
    {
      "id": "309",
      "description": "Zinc, Zn",
      "nutritionValue": 5.2,
      "units": "mg"
    }
  ],
  "servings": [
    {
      "amount": 1,
      "description": "cup (1 NLEA serving)",
      "weightInGrams": 29
    }
  ]
}
```

> [!NOTE]
> Sistem yukarıdaki belgenin boyutu 1 KB biraz değerinden hesaplanır şekilde belgeler Azure Cosmos DB'de küçültülmüş.
> 
> 

Aşağıdaki tabloda yaklaşık istek birimi giderleri (yaklaşık istek birimi ücret hesabı tutarlılık düzeyi "Oturumuna" olarak ayarlandığından ve tüm öğeler otomatik olarak dizine varsayar) öğenin normal işlemleri için gösterilir:

| İşlem | İstek birimi ücret |
| --- | --- |
| Öğesi oluşturma |~15 RU |
| Öğe Okuma |~1 RU |
| Sorgu öğesi kimliği |~2.5 RU |

Ayrıca, bu tabloda yaklaşık istek birimi ücretleri uygulamada kullanılan tipik sorguları için gösterilir:

| Sorgu | İstek birimi ücret | Döndürülen öğe sayısı |
| --- | --- | --- |
| Kimliğe göre yemek seçin |~2.5 RU |1 |
| Üretici tarafından foods seçin |~7 RU |7 |
| Yemek grup ve sipariş ağırlığa göre seçin |~70 RU |100 |
| Üst 10 foods yemek grubunda seçin |~10 RU |10 |

> [!NOTE]
> RU ücretleri döndürülen öğe sayısını göre farklılık gösterir.
> 
> 

Bu bilgi ile operations ve saniye başına beklediğiniz sorguları sayısını verilen bu uygulama için RU gereksinimlerini tahmin edebilirsiniz:

| İşlem/sorgu | Saniye başına tahmini sayısı | Gerekli RUs |
| --- | --- | --- |
| Öğesi oluşturma |10 |150 |
| Öğe Okuma |100 |100 |
| Üretici tarafından foods seçin |25 |175 |
| Yemek gruplandırma ölçütü seçin |10 |700 |
| İlk 10 seçin |15 |150 toplam |

Bu durumda, bir ortalama verimi gereksinimi 1,275 RU/s bekler.  Yuvarlama kadar yakın 100, bu uygulamanın kapsayıcısı için 1300 RU/s sağlamak.

## <a id="RequestRateTooLarge"></a> Azure Cosmos DB aşan ayrılmış işleme sınırları
İstek birimi tüketim bütçe boşsa, saniye başına oranı olarak değerlendirilir, geri çağırma. Hızı ayrılmış düzeyin altına düşene kadar kapsayıcı için sağlanan istek birimi hızı aşan uygulamalar için bu kapsayıcı isteklerine kısıtlanan. Bir kısıtlama oluştuğunda sunucunun erken önlem RequestRateTooLargeException (HTTP durum kodu 429) istekle sona erer ve kullanıcı reattempting önce beklemesi gereken milisaniye cinsinden süreyi belirten x-ms-yeniden deneme-sonra-ms üstbilgi döndürür İstek.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

.NET İstemci SDK'sını ve LINQ sorgularını sonra hiçbir zaman sahip geçerli sürümü .NET İstemci SDK'sı, bu yanıt örtük olarak yakalar gibi bu özel durumu ile mücadele etmek için çoğunlukla kullanıyorsanız, sunucu tarafından belirtilen yeniden deneme sonrasında üstbilgi dikkate alır ve yeniden deneme sayısı İstek. Hesabınızı eşzamanlı olarak birden çok istemci tarafından erişilen sürece, sonraki yeniden deneme işlemi başarılı olur.

İstek hızı işletim üst üste birden fazla istemciniz varsa varsayılan yeniden deneme davranışı değil yeterli olacaktır ve istemci uygulamaya 429 durum koduyla bir DocumentClientException atar. Bu gibi durumlarda, yeniden deneme davranışı ve yordamları işleme veya kapsayıcı için ayrılmış verimliliği artırma uygulamanızın hata mantığının işleme düşünebilirsiniz.

## <a id="RequestRateTooLargeAPIforMongoDB"></a> MongoDB API aşan ayrılmış işleme sınırları
Hızı ayrılmış düzeyin altına düşene kadar kapsayıcı için sağlanan istek birimleri aşan uygulamaları kısıtlanacak. Bir azaltma ortaya çıktığında, arka uç istekle erken önlem sona erer bir *16500* hata kodu - *çok fazla istek*. Varsayılan olarak, MongoDB API otomatik olarak en fazla 10 kez döndürmeden önce yeniden deneme bir *çok fazla istek* hata kodu. Birçok alıyorsanız *çok fazla istek* hata kodları, uygulamanızın hata yordamları işlemedeki ekleme ya da yeniden deneme davranışı dikkate veya [kapsayıcıiçinayrılmışverimliliğiartırma](set-throughput.md).

## <a name="next-steps"></a>Sonraki adımlar
Ayrılmış işleme ile Azure Cosmos DB veritabanları hakkında daha fazla bilgi edinmek için şu kaynakları araştırın:

* [Cosmos DB Azure fiyatlandırması](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Azure Cosmos veritabanı veri bölümlendirme](partition-data.md)

Azure Cosmos DB hakkında daha fazla bilgi için Azure Cosmos DB bkz [belgelerine](https://azure.microsoft.com/documentation/services/cosmos-db/). 

Ölçek ve performans testi Azure Cosmos DB ile kullanmaya başlamak için bkz: [performans ve ölçek testi Azure Cosmos DB ile](performance-testing.md).

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png
[6]: ./media/request-units/api-for-mongodb-metrics.png
