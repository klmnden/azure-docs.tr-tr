---
title: İstek birimleri ve verimlilik - Azure Cosmos DB tahmin etme | Microsoft Docs
description: Anlamak, belirtin ve Azure Cosmos veritabanı istek birimi gereksinimlerini tahmin etme hakkında bilgi edinin.
services: cosmos-db
author: SnehaGunda
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 05/07/2018
ms.author: rimman
ms.openlocfilehash: 16ccda120aef0aa892bf365403f3f0bdc1209ca3
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/06/2018
ms.locfileid: "34823732"
---
# <a name="request-units-in-azure-cosmos-db"></a>Birimleri Azure Cosmos veritabanı isteği

[Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/) Microsoft Genel dağıtılmış multimodel veritabanıdır. Azure Cosmos DB ile sanal makineleri kiralamak, yazılım dağıtma veya veritabanlarını izleme gerekmez. Azure Cosmos DB işletilen ve sürekli olarak dünya çapındaki kullanılabilirliği, performansı ve veri koruma sağlamak üzere Microsoft üst mühendisleri tarafından izlenir. Tercih ettiğiniz API'leri gibi kullanarak, verilerinizi erişebilirsiniz [SQL](documentdb-introduction.md), [MongoDB](mongodb-introduction.md), ve [tablo](table-introduction.md) API'leri ve grafik aracılığıyla [Gremlin API](graph-introduction.md). Tüm API'leri tüm yerel olarak desteklenir. 

Azure Cosmos DB para birimi *istek birimi (RU)*. İstek birimleri ile okuma/yazma kapasiteleri veya sağlama CPU, bellek ve IOPS ayırmak gerekmez. Azure Cosmos DB farklı işlemler sahip çeşitli API'ler destekler, basitten arasında değişen, okur ve karmaşık grafik sorguları yazar. Tüm istekleri eşit olduğundan istek isteğe hizmet vermek için gerekli hesaplama miktarına göre istek birimlerinin normalleştirilmiş bir miktar atanır. Bir işlem için istek birimleri belirleyici sayısıdır. Bir yanıt üstbilgisi aracılığıyla Azure Cosmos veritabanı herhangi bir işlem tarafından kullanılan istek birim sayısını izleyebilirsiniz. 

Tahmin edilebilir performans sağlamak için işleme birimi 100 RU/saniye ayırın. Yapabilecekleriniz [, üretilen iş gereken tahmin](request-units.md#estimating-throughput-needs) Azure Cosmos DB kullanarak [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).

![Üretilen iş hesaplayıcısı][5]

Bu makaleyi okuduktan sonra aşağıdaki soruları yanıtlayın mümkün olacaktır:

* İstek birimleri ve Azure Cosmos DB isteği ücretlere nelerdir?
* Azure Cosmos DB'de nasıl bir kapsayıcı veya kapsayıcıları kümesi için istek birimi kapasitesini belirtin?
* My uygulamanın istek birimi gereken nasıl tahmin?
* Bir kapsayıcı veya Azure Cosmos DB kapsayıcılarında kümesi için istek birim kapasitesi aşmanız durumunda ne olur?

Azure Cosmos DB multimodel veritabanı olduğundan, bu makalenin tüm veri modelleri ve Azure Cosmos DB API'leri uygulanabilir olduğunu dikkate almak önemlidir. Bu makalede gibi genel koşulları kullanan *kapsayıcı* genel bir koleksiyon ya da grafik başvurmak için ve *öğesi* genel olarak bir tablo, belge, düğüm veya varlık başvurmak için.

## <a name="request-units-and-request-charges"></a>İstek birimleri ve istek ücretleri

Azure Cosmos DB hızlı, uygulamanızın verimlilik ihtiyaçlarını karşılamak için kaynakları ayrılarak tahmin edilebilir performans sunar. Uygulama yük ve erişim desenleri zamanla değiştirin. Azure Cosmos DB kolayca artırma veya uygulamanız için kullanılabilir ayrılmış işleme miktarını azaltmak yardımcı olabilir.

Azure Cosmos DB ile ayrılmış işleme saniyede işlediği istek birimi cinsinden belirtilir. İstek birimleri verimlilik para birimi olarak düşünebilirsiniz. Saniye başına temelinde uygulamanız için kullanılabilir olmasını garanti edilen isteği birim sayısını ayırın. Azure Cosmos DB, bir belge yazma dahil olmak üzere her bir işlemde bir sorgu gerçekleştirmek ve bir belge güncelleştirme, CPU, bellek ve IOPS kullanır. Diğer bir deyişle, her işlem istek birimleri cinsinden ifade edilen bir istek ücret doğurur. İstek birimi ücretleri ve uygulamanızın işleme gereksinimleri etkileyen faktörler anladığınızda, uygulamanızı maliyeti etkili bir şekilde olabildiğince olarak çalıştırabilirsiniz. 

Başlamanıza yardımcı olmak için istek birimleri aşağıdaki videoda Azure Cosmos DB Program Yöneticisi Barış Liu ele alınmıştır: <br /><br />

> [!VIDEO https://www.youtube.com/embed/stk5WSp5uX0]
> 
> 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Genel olarak dağıtılmış veritabanlarında işleme yalıtımı

Veritabanınız için birden fazla bölge çoğaltma yaparsanız Azure Cosmos DB istek birimi kullanımına bir bölgede başka bir bölgede istek birimi kullanımına etkilemez emin olmak için işleme yalıtım sağlar. Örneğin, tek bir bölge için veri yazma ve başka bir bölgeye verileri okumak, bir bölgede yazma işlemi gerçekleştirmek için kullanılan istek birimleri ele yok B. isteği bölgede okuma işlemi için kullanılan istek birimleri çıktığınızda birimleri eri ayırın değil ss veritabanınızı, dağıttığınız bölgeleri. Veritabanı çoğaltılan her bölge sağlanan istek birimleri tam sayısına sahip. Genel çoğaltma hakkında daha fazla bilgi için bkz: [Azure Cosmos DB genel verilerle dağıtmak nasıl](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>İstek birimi konuları
Sağlama isteği birim sayısını tahmin yaparken, aşağıdaki değişkenleri dikkate almanız önemlidir:

* **Öğe boyutunu**. Boyutu arttıkça okumak veya veri yazmak için kullanılan istek birimi sayısı da artar.
* **Madde özellik sayısı**. Tüm özelliklerin, belge, düğüm veya varlık bir artış özellik sayısı arttıkça yazmak için kullanılan birimleri varsayılan dizin varsayılır.
* **Veri tutarlılığı**. Veri tutarlılığı modelleri güçlü veya sınırlanmış eskime durumu gibi kullandığınızda, ek istek birimleri öğeleri okumak için kullanılır.
* **Dizin oluşturulmuş özellikleri**. Bir dizin İlkesi her kapsayıcısı üzerinde varsayılan olarak hangi özellikleri dizinlenir belirler. Dizinli Özellikler sayısını sınırlayarak veya yavaş dizin etkinleştirerek yazma işlemleri için istek birimi tüketimini azaltabilir.
* **Belge dizine**. Varsayılan olarak, her bir öğeyi otomatik olarak dizine alınır. Bazı öğelerinizi dizin kullanılamıyor seçerseniz daha az istek birimi kullanabilir.
* **Sorgu desenleri**. Bir sorgu karmaşıklığını kaç tane istek birimi için bir işlem tüketilen etkiler. Koşulları, koşulları yapısını, kullanıcı tanımlı işlevler sayısı, kaynak verilerin boyutunu sayısı sorgu sayısı, sonuçları ve sorgu işlemlerinin maliyetini tahminleri tüm etkiler.
* **Komut dosyası kullanımı**. Sorguları olarak gerçekleştirilen işlemler kapsamına bağlı istek birimleri saklı yordamları ve Tetikleyicileri tüketir. Uygulamanızı geliştirirken, her işlem isteği birim kapasitesi nasıl tüketir daha iyi anlamak için istek ücret üstbilgisi inceleyin.

## <a name="estimating-throughput-needs"></a>Üretilen iş gereksinimlerini tahmin etme
Bir istek birimi istek maliyet işleme normalleştirilmiş ölçüsüdür. Bir tek istek birimi (kendi bağlantısını veya kimliği), 10 benzersiz özellik değerlerini (Sistem özellikleri dışında) oluşan tek bir 1 KB öğe okumak için gerekli işlem kapasitesini temsil eder. (Ekle) oluşturma isteği değiştirmek veya aynı silme öğesi hizmetinden daha fazla işleme kullanır ve böylece daha fazla istek birimi gerektirir. 

> [!NOTE]
> Temel bir 1 KB öğesi için 1 isteği biriminin kendi bağlantısını basit bir GET veya öğenin kimliği karşılık gelir.
> 
> 

Örneğin, sağlama öğeleri üç farklı boyutlarda (1 KB, 4 KB ve 64 KB) ile ve iki farklı performans düzeyleri için kaç tane istek birimleri gösteren bir tablo İşte (500 Okuma/saniye 100 yazma/saniye ve 500 Okuma/saniye + 500 yazma/saniye). Bu örnekte, veri tutarlılığını ayarlamak **oturum**, ve dizin oluşturma ilkesini ayarlamak **hiçbiri**.

| Öğesi boyutu | Okuma/saniye | Yazma/saniye | İstek birimleri
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) = 1.000 RU/s
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) = 3000 RU/s
| 4 KB | 500 | 100 | (500 * 1,3) + (100 * 7) = 1,350 RU/s
| 4 KB | 500 | 500 | (500 * 1,3) + (500 * 7) = 4,150 RU/s
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9,800 RU/s
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) = 29,000 RU/s


### <a name="use-the-request-unit-calculator"></a>İstek birimi hesaplayıcı kullanın
Üretilen iş tahminler ince ayar yardımcı olmak için web tabanlı kullanabileceğiniz [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner). Hesaplayıcı, tahmin isteği birim gereksinimleri dahil olmak üzere, normal işlemleri için yardımcı olabilir:

* (Yazar) öğesi oluşturur
* Öğe okur
* Öğeyi siler
* Öğesi güncelleştirmeleri

Aracı, sağladığınız örnek öğeleri temel alan veri depolama gereksinimlerini tahmin etmek için destek de içerir.

Aracı kullanmak için:

1. Bir veya daha fazla temsilcisi öğeleri (örneğin, bir örnek JSON belgesi) karşıya yükleyin.
   
    ![İstek birimi makinesine öğeleri karşıya yükle][2]
2. Veri depolama gereksinimlerini tahmin etmek için depolamayı beklediğiniz öğeleri (örneğin, belgeler, satır veya köşeleri) toplam sayısı girin.
3. Oluşturma, okuma, güncelleştirme ve ihtiyaç duyduğunuz silme işlemlerinin sayısı (bir saniye başına temelinde) girin. İstek birimi ücretleri öğesi güncelleştirme işlemlerinin tahmin etmek için 1. adımdaki tipik alan güncelleştirmeleri içeren örnek öğenin bir kopyasını yükleyin. Örneğin, öğesi güncelleştirmeler genellikle adlı iki özelliklerini değiştirmek *lastLogin* ve *userVisits*örnek öğesi kopyalama, bu iki özellik için değerleri güncelleştirmek ve kopyalanan öğeyi karşıya yükleyin.
   
    ![Üretilen iş gereksinimleri istek birimi hesaplayıcı girin][3]
4. Seçin **Hesapla**ve ardından sonuçları inceleyin.
   
    ![Birim hesaplayıcı sonuçları isteği][4]

> [!NOTE]
> Önemli ölçüde boyutu ve Dizinli Özellikler sayısı bakımından farklılık gösterir öğesi türleriniz varsa, her bir örnek karşıya *türü* tipik öğesi için araç ve sonuçları hesaplar.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Azure Cosmos DB istek ücret yanıt üstbilgisi kullanın
Her yanıt Azure Cosmos DB hizmetinden bir özel üst bilgi içeriyor (`x-ms-request-charge`) belirli bir istek için kullanılan istek birimleri içerir. Bu üst Azure Cosmos DB SDK'ları da erişebilirsiniz. .NET SDK'sındaki **RequestCharge** bir özelliktir **ResourceResponse** nesnesi. Sorgular için Azure portalında Azure Cosmos DB Veri Gezgini yürütülen sorgular için istek ücret bilgileri sağlar.

Uygulamanızın gerektirdiği ayrılmış işleme miktarı tahmin etmek için bir yöntem, uygulamanız tarafından kullanılan bir temsili öğesi karşı normal işlemleri çalıştırılması ile ilişkili istek birimi ücret kaydetmektir. Ardından, her saniye gerçekleştirmek düşündüğünüz işlemlerinin sayısını tahmin edin. Ayrıca, ölçü ve tipik sorgular ve Azure Cosmos DB komut dosyası kullanımı dahil emin olun.

> [!NOTE]
> Önemli ölçüde boyutu ve Dizinli Özellikler sayısı bakımından farklılık gösterir öğesi türleriniz varsa, her ilişkilendirilmiş geçerli işlem istek birimi ücret kayıt *türü* tipik öğesi.
> 
> 

Örneğin, bunlar adımlar şunlardır:

1. (Ekleme) oluşturma isteği birim ücret kayıt tipik bir öğe. 
2. Tipik bir öğe Okuma isteği birim ücret kaydedin.
3. Tipik bir öğesi güncelleştirme isteği birim ücret kaydedin.
4. Tipik, ortak öğesi sorgularını istek birimi olarak kaydedin.
5. Uygulamanın kullandığı tüm özel komut dosyalarını (saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler) istek birimi olarak kaydedin.
6. Saniyede çalıştırmayı düşündüğünüz operations tahmini sayısını verilen gerekli istek birimleri hesaplayın.

## <a name="a-request-unit-estimate-example"></a>Bir istek birimi tahmin örneği
Yaklaşık 1 KB boyutunda aşağıdaki belge göz önünde bulundurun:

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
> Yukarıdaki belgenin sistem tarafından hesaplanan boyutu 1 KB biraz daha az olacak şekilde belgeler Azure Cosmos DB'de küçültülmüş.
> 
> 

Aşağıdaki tabloda, bu öğeyi normal işlemleri için yaklaşık istek birimi ücretleri gösterir. (Hesabı tutarlılık düzeyi ayarlamak yaklaşık istek birimi ücret varsayar **oturum** ve tüm öğeler otomatik olarak dizini oluşturulmuş.)

| İşlem | İstek birimi ücret |
| --- | --- |
| Öğe oluştur |~15 RU |
| Öğe Okuma |~ 1 RU |
| Sorgu öğesi kimliği |~2.5 RU |

Aşağıdaki tabloda yaklaşık istek birimi ücretleri uygulamada kullanılan tipik sorguları için gösterilir:

| Sorgu | İstek birimi ücret | döndürülen öğe sayısı |
| --- | --- | --- |
| Kimliğe göre yemek seçin |~2.5 RU |1 |
| Üretici tarafından foods seçin |~7 RU |7 |
| Yemek grup ve sipariş ağırlığa göre seçin |~70 RU |100 |
| Üst 10 foods yemek grubunda seçin |~ 10 RU |10 |

> [!NOTE]
> İstek birimi ücretleri döndürülen öğe sayısını göre farklılık gösterir.
> 
> 

Bu bilgi ile işlemler ve saniye başına beklediğiniz sorguları sayısı verilen bu uygulama için istek birimi gereksinimlerini tahmin edebilirsiniz:

| İşlem/sorgu | Saniye başına tahmini sayısı | Gerekli istek birimleri |
| --- | --- | --- |
| Öğe oluştur |10 |150 |
| Öğe Okuma |100 |100 |
| Üretici tarafından foods seçin |25 |175 |
| Yemek gruplandırma ölçütü seçin |10 |700 |
| İlk 10 seçin |15 |150 toplam |

Bu durumda, bir ortalama işleme gereksinimini 1,275 RU/saniye bekler. Yuvarlama kadar yakın 100, 1,300 RU/saniye için bu uygulamanın kapsayıcı (veya kapsayıcıları kümesi) sağlamak.

## <a id="RequestRateTooLarge"></a> Azure Cosmos DB aşan ayrılmış işleme sınırları
İstek birimi tüketim saniyede hızında değerlendirilir. Hızı sağlanan işleme düzeyin altına düşene kadar sağlanan istek birimi hızı aşan uygulamalar için istek oranı sınırlı. Bir istek oranı sınırlı olduğunda, sunucu erken önlem istekle sona `RequestRateTooLargeException` (HTTP durum kodu 429) ve döndürür `x-ms-retry-after-ms` üstbilgi. Üstbilgi Kullanıcı isteği yeniden denemeden önce beklemesi gereken milisaniye cinsinden süre miktarını gösterir.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

.NET İstemci SDK'sını ve LINQ sorgularını kullan çoğu zaman, hiçbir zaman bu özel durumla dağıtılacak varsa. Geçerli sürümü .NET İstemci SDK'sı, bu yanıt örtük olarak yakalar, yeniden deneme sonra sunucu tarafından belirtilen üstbilgi dikkate alır ve istek otomatik olarak yeniden dener. Hesabınızı eşzamanlı olarak birden çok istemci tarafından erişilen sürece, sonraki yeniden deneme işlemi başarılı olur.

Birden fazla istemci isteği hızı üst üste işletim varsa, varsayılan yeniden deneme davranışı yetersiz olabilir ve istemci oluşturur bir `DocumentClientException` uygulamaya 429 ile durum kodu. Bu gibi durumlarda yeniden deneme davranışı ve uygulamanızın hata işleme yordamları mantığı işleme göz önünde bulundurun veya kapsayıcı (veya kapsayıcıları kümesi) için sağlanan verimliliğini artırmak isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
 
- Bilgi nasıl [ayarlama ve verimlilik Azure Cosmos DB'de alma](set-throughput.md) Azure portal ve SDK'ları kullanarak.
- Hakkında bilgi edinin [performansı ve ölçeği Azure Cosmos DB ile test](performance-testing.md).
- Ayrılmış işleme ile Azure Cosmos DB veritabanları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/) ve [Azure Cosmos veritabanı veri bölümlendirme](partition-data.md).
- Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB belgeleri](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png

