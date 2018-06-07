---
title: Birimleri iste & verimlilik - Azure Cosmos DB tahmin etme | Microsoft Docs
description: Anlamak, belirtin ve Azure Cosmos veritabanı istek birimi gereksinimlerini tahmin etme hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: b8084008089225c11c8052c60be3afc152881040
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/01/2018
ms.locfileid: "34614841"
---
# <a name="request-units-in-azure-cosmos-db"></a>Azure Cosmos DB birimlerinde isteği

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Microsoft'un Genel dağıtılmış birden çok model veritabanıdır. Azure Cosmos DB ile sanal makineleri kiralamak, yazılım dağıtma veya veritabanlarını izleme gerekmez. Azure Cosmos DB işletilen ve sürekli olarak world sınıfı kullanılabilirliği, performansı ve veri koruma sağlamak üzere Microsoft üst mühendisleri tarafından izlenir. Tercih ettiğiniz API'leri gibi kullanarak, verilerinizi erişebilirsiniz [SQL API](documentdb-introduction.md), [MongoDB API](mongodb-introduction.md), [tablo API](table-introduction.md)ve aracılığıyla grafik [Gremlin API](graph-introduction.md) - tüm yerel olarak desteklenir. 

Azure Cosmos DB para birimi **istek birimi (RU)**. RUs ile okuma/yazma kapasiteleri veya sağlama CPU, bellek ve IOPS ayırmak gerekmez. Azure Cosmos DB Basit okuma arasında değişen farklı işlemlerle API'lerini destekler ve karmaşık grafik sorguları yazar. Tüm istekleri eşit olduğundan, normalleştirilmiş bir miktar atanan **istek birimleri** isteğe hizmet vermek için gerekli hesaplama miktarına göre. Bir işlem için istek birim sayısı belirleyici ve bir yanıt üstbilgisi aracılığıyla Azure Cosmos veritabanı herhangi bir işlem tarafından kullanılan istek birim sayısını izleyebilirsiniz. 

Tahmin edilebilir performans sağlamak için 100 RU/saniye cinsinden aktarım hızı ayırmanız gerekir. Yapabilecekleriniz [, üretilen iş gereken tahmin](request-units.md#estimating-throughput-needs) Azure Cosmos DB kullanarak [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).

![Üretilen iş hesaplayıcısı][5]

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:  

* İstek birimleri ve Azure Cosmos DB isteği ücretlere nelerdir?
* Azure Cosmos DB'de nasıl bir kapsayıcı veya kapsayıcıları kümesi için istek birimi kapasitesini belirtin?
* My uygulamanın istek birimi gereken nasıl tahmin?
* Bir kapsayıcı veya Azure Cosmos DB kapsayıcılarında kümesi için istek birim kapasitesi aşmanız durumunda ne olur?

Azure Cosmos DB çok model veritabanı olarak; Bu makalede tüm veri modelleri ve Azure Cosmos DB API'leri uygulanabilir olduğunu dikkate almak önemlidir. Bu makalede genel koşulları gibi kullanan *kapsayıcı* ve bir *öğesi* genel bir koleksiyon, grafik veya bir tablo ve bir belge, bir düğüm veya bir varlık için sırasıyla başvurmak için.

## <a name="request-units-and-request-charges"></a>İstek birimleri ve istek ücretleri
Azure Cosmos DB tarafından hızlı ve tahmin edilebilir performans sunar *ayırma* uygulamanızın verimlilik gereken karşılamak için kaynakları.  Uygulama yüklemek ve zaman içinde desenleri değişiklik erişmek için Azure Cosmos DB kolayca artırın veya uygulamanız için kullanılabilir ayrılmış işleme miktarını azaltmak sağlar.

Azure Cosmos DB ile ayrılmış işleme saniyede işlediği istek birimler cinsinden belirtilir. İstek birimleri yapabildiği verimlilik para birimi olarak düşünebilirsiniz, *yedek* garantili istek birimleri uygulamanıza kullanılabilir miktardaki saniye başına temelinde.  Azure Cosmos - bir belge yazma, bir belge güncelleştirme bir sorgu gerçekleştirme - DB her bir işlemin CPU, bellek ve IOPS tüketir.  Diğer bir deyişle, her işlemi uygulanan bir *isteği ücret*, içinde ifade *istek birimleri*.  İstek birimi ücretleri, uygulamanızın işleme gereksinimleri etkileyen faktörler anlama, uygulamanızın maliyeti etkili bir şekilde olabildiğince olarak çalıştırmak sağlar. Azure portalında Veri Gezgini ayrıca bir sorgu çekirdek test etmek için bir harika aracıdır.

Burada Azure Cosmos DB Program Yöneticisi Barış Liu istek birimleri anlatılmaktadır aşağıdaki videoyu izleyerek çalışmaya başlamanızı öneririz.

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Genel olarak dağıtılmış veritabanlarında işleme yalıtımı

Birden fazla bölgeye veritabanınızı çoğaltıldığından, Azure Cosmos DB RU kullanım bir bölgede başka bir bölgede RU kullanım etkilememesini sağlamak için işleme yalıtım sağlar. Tek bir bölge için veri yazma ve başka bir bölgesinden veri okuma, örneğin, RUs bölgede yazma işlemi gerçekleştirmek için kullanılan *A* bölgede okuma işlemi için kullanılan RUs çıktığınızda almayan *B*. RUs dağıttıktan sonra bölgeler arasında bölünür değil. Veritabanı çoğaltılan her bölge sağlanan RUs tam sayısına sahip. Genel çoğaltma hakkında daha fazla bilgi için bkz: [Azure Cosmos DB genel verilerle dağıtmak nasıl](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>İstek birimi konuları
Sağlama isteği birim sayısını tahmin yaparken, aşağıdaki değişkenleri dikkate önemlidir:

* **Öğe boyutunu**. İstek birimleri okumak veya veri yazmak için kullanılan boyutu sayısı arttıkça da artırır.
* **Madde özellik sayısı**. Tüm özelliklerin, bir belge/varlık başına düğüm artış özellik sayısı arttıkça yazmak için kullanılan birimleri varsayılan dizin varsayılır.
* **Veri tutarlılığı**. Veri tutarlılığı modelleri güçlü veya sınırlanmış eskime durumu gibi kullanırken, ek istek birimleri öğeleri okumak için kullanılır.
* **Dizin oluşturulmuş özellikleri**. Bir dizin İlkesi her kapsayıcısı üzerinde varsayılan olarak hangi özellikleri dizinlenir belirler. Dizinli Özellikler sayısını sınırlayarak veya yavaş dizin etkinleştirerek yazma işlemleri için istek birimi tüketimini azaltabilir.
* **Belge dizine**. Varsayılan olarak, her bir öğeyi otomatik olarak dizine alınır. Bazı öğelerinizi dizin kullanılamıyor seçerseniz daha az istek birimi kullanabilir.
* **Sorgu desenleri**. Bir sorgu karmaşıklığını kaç tane istek birimi için bir işlem tüketilen etkiler. Sorgu sonuçları sayısını, koşulları sayısı, koşulları, projeksiyonları, UDF'ler sayısı ve kaynak verileri - boyutunu yapısını tüm sorgu işlemlerinin maliyetini etkiler.
* **Komut dosyası kullanımı**.  Sorguları olarak gerçekleştirilen işlemler kapsamına bağlı istek birimleri saklı yordamları ve Tetikleyicileri tüketir. Uygulamanızı geliştirirken, her işlem isteği birim kapasitesi nasıl tüketen daha iyi anlamak için istek ücret üstbilgisi inceleyin.

## <a name="estimating-throughput-needs"></a>Üretilen iş gereksinimlerini tahmin etme
Bir istek birimi istek maliyet işleme normalleştirilmiş ölçüsüdür. Bir tek istek birimi (kendi bağlantısını veya kimliği) öğesi 10 benzersiz özellik değerlerini (Sistem özellikleri dışında) oluşan tek 1 bb okumak için gereken işlem kapasitesi temsil eder. Daha fazla hizmet ve böylece işleme daha fazla istek birimi (Ekle) oluşturmak, değiştirmek veya aynı öğeyi silmek için bir istek tüketir.   

> [!NOTE]
> 1 istek birimi bir 1 KB öğesi için taban çizgisi için basit bir GET kendi bağlantısını veya öğenin kimliği tarafından karşılık gelir.
> 
> 

Örneğin, sağlama öğeleri üç farklı boyutlarda (1 KB, 4 KB ve 64 KB) ile ve iki farklı performans düzeyleri için kaç tane istek birimleri gösteren bir tablo İşte (500 Okuma/saniye 100 yazma/saniye ve 500 Okuma/saniye + 500 yazma/saniye). Veri tutarlılığı sırasında yapılandırılan *oturum*, ve dizin oluşturma ilkesini ayarlandı *hiçbiri*.

<table border="0" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td valign="top"><p><strong>Öğesi boyutu</strong></p></td>
            <td valign="top"><p><strong>Okuma/saniye</strong></p></td>
            <td valign="top"><p><strong>Yazma/saniye</strong></p></td>
            <td valign="top"><p><strong>İstek birimleri</strong></p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1) + (100 * 5) = 1.000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>1 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1) + (500 * 5) = 3000 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>100</p></td>
            <td valign="top"><p>(500 * 1,3) + (100 * 7) = 1,350 RU/s</p></td>
        </tr>
        <tr>
            <td valign="top"><p>4 KB</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>500</p></td>
            <td valign="top"><p>(500 * 1,3) + (500 * 7) = 4,150 RU/s</p></td>
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

1. Bir veya daha fazla temsilcisi öğeleri (örneğin, bir örnek JSON belgesi) karşıya yükleyin.
   
    ![İstek birimi makinesine öğeleri karşıya yükle][2]
2. Veri depolama gereksinimlerini tahmin etmek için toplam sayısını girin (örneğin, belgeler, satır veya köşeleri) öğelerinin depolamayı beklediğiniz.
3. Oluşturma, okuma, güncelleştirme ve silme işlemleri gerektirir (bir saniye başına temelinde) girin. İstek birimi ücretleri öğesi güncelleştirme işlemlerinin tahmin etmek için 1. adımdaki tipik alan güncelleştirmeleri içeren Yukarıdaki örnek öğenin bir kopyasını yükleyin.  Örneğin, öğesi güncelleştirmeler genellikle adlı iki özelliklerini değiştirmek *lastLogin* ve *userVisits*, ardından örnek öğesi kopyalama, bu iki özellik için değerleri güncelleştirmek ve kopyalanan öğeyi karşıya yükleyin.
   
    ![Üretilen iş gereksinimleri istek birimi hesaplayıcı girin][3]
4. Tıklatın hesaplamak ve sonuçları inceleyin.
   
    ![Birim hesaplayıcı sonuçları isteği][4]

> [!NOTE]
> Boyutu ve Dizinli Özellikler sayısı bakımından önemli ölçüde farklılık gösterecektir öğesi türleri varsa, her bir örnek karşıya yükleme *türü* tipik öğesi için araç ve sonuçları hesaplayın.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Azure Cosmos DB istek ücret yanıt üstbilgisi kullanın
Her yanıt Azure Cosmos DB hizmetinden bir özel üst bilgi içeriyor (`x-ms-request-charge`) belirli bir istek için kullanılan istek birimleri içerir. Bu üst ayrıca Azure Cosmos DB SDK erişilebilir. .NET SDK'sındaki `RequestCharge` bir özelliktir `ResourceResponse` nesnesi.  Sorgular için Azure portalında Azure Cosmos DB Veri Gezgini yürütülen sorgular için istek ücret bilgileri sağlar.

Bu durum dikkate alınarak, uygulamanızın gerektirdiği ayrılmış işleme miktarı tahmin etmek için bir uygulamanız tarafından kullanılan bir temsili öğesi karşı normal işlemleri çalıştırılması ile ilişkili istek birimi ücret kaydedin ve ardından tahmin yöntemdir saniyede gerçekleştirmek düşündüğünüz işlemlerinin sayısı.  Ölçmek ve tipik sorgular ve Azure Cosmos DB komut dosyası kullanımı da dahil emin olun.

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

## <a name="a-request-unit-estimate-example"></a>Bir istek birimi tahmin örneği
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

Aşağıdaki tabloda, bu öğeyi normal işlemleri için birim ücretleri yaklaşık isteği gösterilmektedir (hesap tutarlılık düzeyi ayarlamak yaklaşık istek birimi ücret varsayar *oturum* ve tüm öğeler otomatik olarak olan Dizinli):

| İşlem | İstek birimi ücret |
| --- | --- |
| Öğe oluştur |~15 RU |
| Öğe Okuma |~ 1 RU |
| Sorgu öğesi kimliği |~2.5 RU |

Ayrıca, bu tabloda yaklaşık istek birimi ücretleri uygulamada kullanılan tipik sorguları için gösterilir:

| Sorgu | İstek birimi ücret | Döndürülen öğe sayısı |
| --- | --- | --- |
| Kimliğe göre yemek seçin |~2.5 RU |1 |
| Üretici tarafından foods seçin |~7 RU |7 |
| Yemek grup ve sipariş ağırlığa göre seçin |~70 RU |100 |
| Üst 10 foods yemek grubunda seçin |~ 10 RU |10 |

> [!NOTE]
> RU ücretleri döndürülen öğe sayısını göre değişir.
> 
> 

Bu bilgi ile operations ve saniye başına beklediğiniz sorguları sayısını verilen bu uygulama için RU gereksinimlerini tahmin edebilirsiniz:

| İşlem/sorgu | Saniye başına tahmini sayısı | Gerekli RUs |
| --- | --- | --- |
| Öğe oluştur |10 |150 |
| Öğe Okuma |100 |100 |
| Üretici tarafından foods seçin |25 |175 |
| Yemek gruplandırma ölçütü seçin |10 |700 |
| İlk 10 seçin |15 |150 toplam |

Bu durumda, bir ortalama verimi gereksinimi 1,275 RU/s bekler.  Yuvarlama kadar yakın 100, bu uygulamanın kapsayıcı (veya kapsayıcıları kümesi) 1300 RU/s sağlamak.

## <a id="RequestRateTooLarge"></a> Azure Cosmos DB aşan ayrılmış işleme sınırları
İstek birimi tüketim saniyede bir hızda değerlendirilir çağırma. Sağlanan istek birimi hızı aşan uygulamalar için oranı sağlanan işleme düzeyin altına düşene kadar istekleri oranı sınırlı olacaktır. Bir istek oranı sınırlı alır, sunucunun erken önlem istekle sona erer `RequestRateTooLargeException` (HTTP durum kodu 429) ve döndürür `x-ms-retry-after-ms` Kullanıcı isteği yeniden denemeden önce beklemesi gereken milisaniye cinsinden süreyi belirten üstbilgi.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

.NET İstemci SDK'sını ve LINQ sorgularını sonra hiçbir zaman sahip geçerli sürümü .NET İstemci SDK'sı, bu yanıt örtük olarak yakalar gibi bu özel durumu ile mücadele etmek için çoğunlukla kullanıyorsanız, sunucu tarafından belirtilen yeniden deneme sonrasında üstbilgi dikkate alır ve yeniden deneme sayısı otomatik olarak isteyin. Hesabınızı eşzamanlı olarak birden çok istemci tarafından erişilen sürece, sonraki yeniden deneme işlemi başarılı olur.

Birden fazla istemci isteği hızı üst üste işletim varsa, varsayılan yeniden deneme davranışı değil yeterli olacaktır ve istemci özel durum oluşturacak bir `DocumentClientException` uygulamaya 429 ile durum kodu. Bu gibi durumlarda yeniden deneme davranışı ve uygulamanızın hata yordamları işleme mantığı işleme göz önünde bulundurun veya kapsayıcı (veya kapsayıcıları kümesi) için sağlanan verimliliğini artırmak isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
 
Ayarlama ve Azure portal ve SDK kullanarak işleme alma hakkında bilgi edinmek için kılavuzuna bakın:

* [Ayarlama ve verimlilik Azure Cosmos DB'de alma](set-throughput.md)

Ayrılmış işleme ile Azure Cosmos DB veritabanları hakkında daha fazla bilgi edinmek için şu kaynakları araştırın:

* [Cosmos DB Azure fiyatlandırması](https://azure.microsoft.com/pricing/details/cosmos-db/)
* [Azure Cosmos veritabanı veri bölümlendirme](partition-data.md)

Azure Cosmos DB hakkında daha fazla bilgi için Azure Cosmos DB bkz [belgelerine](https://azure.microsoft.com/documentation/services/cosmos-db/). 

Ölçek ve performans testi Azure Cosmos DB ile kullanmaya başlamak için bkz: [performans ve ölçek testi Azure Cosmos DB ile](performance-testing.md).

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

