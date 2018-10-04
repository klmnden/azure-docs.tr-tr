---
title: İstek birimleri ve aktarım hızı - Azure Cosmos DB tahmin etme | Microsoft Docs
description: Anlamanıza, belirtin ve Azure Cosmos DB'de istek birimi gereksinimlerini tahmin etme hakkında bilgi edinin.
services: cosmos-db
author: rimman
manager: kfile
ms.service: cosmos-db
ms.devlang: na
ms.topic: conceptual
ms.date: 10/02/2018
ms.author: rimman
ms.openlocfilehash: 23a3e629e12e2a4d417757c9fef5db804bb72c9e
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248763"
---
# <a name="throughput-and-request-units-in-azure-cosmos-db"></a>Azure Cosmos DB'de aktarım hızı ve istek birimleri

Azure Cosmos DB kaynaklarını sağlanan aktarım hızı ve depolama göre faturalandırılır. Azure Cosmos DB aktarım hızı açısından ifade edilir **istek birimi (RU/sn) saniyede**. Azure Cosmos DB, farklı işlem var çeşitli API'ler destekler, basit arasında değişen okur ve karmaşık graf sorgularını yazar. Her isteği istek birimlerini isteğe hizmet vermek için gerekli hesaplama miktarını kullanır. Bir işlem için istek birimi belirleyici sayısıdır. Yanıt üst bilgisini kullanarak kullanılan herhangi bir işlem Azure Cosmos DB'de istek birimleri sayısını takip edebilirsiniz. Tahmin edilebilir performans sağlamak için aktarım hızı birimi 100 RU/saniye ayırmanız gerekir. Azure Cosmos DB kullanarak üretilen iş hacmi gereksinimlerinizi tahmin edebilirsiniz [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner).

Azure Cosmos DB'de aktarım hızını iki ayrıntı düzeylerinde sağlayabilirsiniz: 

1. **Azure Cosmos DB kapsayıcısı:** yalnızca bu belirli kapsayıcı için ayrılmış bir kapsayıcı için sağlanan aktarım hızı. Olarak kapsayıcı düzeyinde Throughput(RU/s) atarken kapsayıcıları oluşturulabilir **sabit** veya **sınırsız**. 

  Sabit boyutlu kapsayıcıların en yüksek aktarım hızı sınırı 10.000 RU/s ve 10 GB depolama sınırı vardır. Sınırsız bir kapsayıcı oluşturmak için en düşük aktarım hızı, 1.000 RU/sn belirtmeniz gerekir ve bir [bölüm anahtarı](partition-data.md). Verilerinizi birden çok bölümde bölünebilir olduğundan, yüksek bir kardinalite (100 milyonlarca ayrı değer) sahip bir bölüm anahtarı seçin gerekir. Birçok farklı değerlere sahip bir bölüm anahtarı'nı seçerek, Azure Cosmos DB koleksiyonu, tablo ve graf isteklerine birörnek ölçeği sağlar. 

2. **Azure Cosmos DB veritabanı:** bu veritabanındaki tüm kapsayıcılar arasında paylaşılan bir veritabanı için sağlanan aktarım hızı. Aktarım hızı ve veritabanı düzeyinde sağlanırken açıkça belirli kapsayıcılarını dışlayın ve bunun yerine kapsayıcı düzeyinde kapsayıcıların verimliliğini sağlamak seçebilirsiniz. Veritabanı düzeyinde aktarım hızını bölüm anahtarıyla oluşturulacak tüm koleksiyonlar gerektirir. Her bir koleksiyon olduğundan aktarım hızı ve veritabanı düzeyinde atarken bu veritabanına ait kapsayıcıları bir bölüm anahtarı ile oluşturulması gereken bir **sınırsız** kapsayıcı.  

Sağlanan aktarım hızına göre Azure Cosmos DB büyüdükçe, kapsayıcılar ve Gruplama verilerinizi bölümler arasında barındırmak için fiziksel bölümlere ayırır. Aşağıdaki görüntüde, farklı düzeylerde sağlama aktarım hızı gösterilmiştir:

  ![İstek birimleri için ayrı kapsayıcıları ve kapsayıcıları kümesi sağlama](./media/request-units/provisioning_set_containers.png)

> [!NOTE] 
> Aktarım hızı kapsayıcı düzeyi ve veritabanı düzeyinde sağlama olan ayrı teklifleri ve ya da bunların arasında geçiş gerektiren veri kaynaktan hedefe geçişi. Yeni bir veritabanı veya yeni bir koleksiyon oluşturun ve ardından verileri kullanarak geçirmek için anlamına gelir [toplu Yürütücü Kitaplığı](bulk-executor-overview.md) veya [Azure Data Factory](../data-factory/connector-azure-cosmos-db.md).

## <a name="request-units-and-request-charges"></a>İstek birimleri ve istek ücretleri

Azure Cosmos DB, uygulamanızın aktarım hızı ihtiyaçlarını karşılamak için gerekli kaynakları ayırarak hızlı ve tahmin edilebilir performans sunar. Uygulama yükü ve erişim düzenleri zaman içinde değişir. Azure Cosmos DB, uygulamanızın kullanabileceği ayrılmış aktarım hızı miktarını kolayca artırmanıza veya azaltmanıza yardımcı olabilir.

Azure Cosmos DB ile ayrılmış aktarım hızı, işleme saniye başına istek birimi cinsinden belirtilir. İstek birimleri aktarım hızı para birimi olarak düşünebilirsiniz. Uygulamanızın saniyelik aralıklarla kullanılabilir olmasını garanti edilen istek birimi sayısı saklı tutarız. Azure Cosmos DB, bir belge yazma dahil olmak üzere her bir işlemde bir sorgu gerçekleştirmek ve bir belge güncelleştiriliyor, CPU, bellek ve IOPS kullanır. Diğer bir deyişle, her işlem, istek birimleri cinsinden ifade edilen bir istek ücret doğurur. İstek birimi ücreti ve uygulamanızın aktarım hızı gereksinimleri etkileyen faktörler anladığınızda, uygulamanızı maliyeti etkili bir şekilde olabildiğince olarak çalıştırabilirsiniz. 

## <a name="throughput-isolation-in-globally-distributed-databases"></a>Global olarak dağıtılmış veritabanları işleme yalıtımı

Veritabanınızın birden fazla bölgeye çoğaltma, Azure Cosmos DB istek birimi kullanımının bir bölgede başka bir bölgede istek birimi kullanımının etkilemediğinden emin olmak için aktarım hızı yalıtım sağlar. Örneğin, bir bölgeye verileri yazmak ve başka bir bölgeye verileri okuyun, bir bölgede yazma işlemi gerçekleştirmek için kullanılan istek birimleri yakalayana b isteği bölgede okuma işlemi için kullanılan istek birimleri uzağa birimleri eri bölme değil ss veritabanınızı içinde dağıttığınız bölgeleri. Her bölge içinde veritabanı çoğaltılır, sağlanan istek birimi tam sayısına sahip. Genel çoğaltma hakkında daha fazla bilgi için bkz. [küresel olarak Azure Cosmos DB ile verileri nasıl dağıtılacağını](distribute-data-globally.md).

## <a name="request-unit-considerations"></a>İstek birimi konuları
İstek birimi sağlamak için tahmini yaparken, aşağıdaki değişkenleri dikkate almak önemlidir:

* **Öğe boyutunu**. Boyutu arttıkça, verileri okuma veya yazma için kullanılan istek birimleri sayısı da artar.
* **Öğe özellik sayısı**. Tüm özelliklerinin bir belge, düğümü veya varlık artış özellik sayısı arttıkça yazmak için kullanılan birimleri varsayılan dizin kullanılır.
* **Veri tutarlılığı**. Tanımlayıcı veya sınırlanmış eskime durumu gibi veri tutarlılık modeli kullandığınız zaman ek istek birimi öğeleri okumak için kullanılır.
* **Dizin oluşturulmuş özellikleri**. Her kapsayıcı bir dizin İlkesi, hangi özellikleri varsayılan olarak dizinlenir belirler. Dizinli Özellikler sayısını sınırlama veya yavaş dizin oluşturmayı etkinleştirmek, yazma işlemleri için istek birimi tüketimini azaltabilirsiniz.
* **Belge dizine**. Varsayılan olarak, her bir öğe otomatik olarak dizine alınır. Değil, öğelerin bazıları dizin seçerseniz istek birimi daha az kullanır.
* **Sorgu desenleri**. Kaç tane istek birimi bir işlem için kullanılan bir sorgu karmaşıklığı etkiler. Koşullar, koşullarına yapısını, kullanıcı tanımlı işlev sayısı, kaynak verilerin boyutunu sayısını sorgu sayısı, sonuçları ve sorgu işlemlerinin maliyetini projeksiyonlar tüm etkiler.
* **Betik kullanımı**. Sorgularla olduğu gibi istek birimleri gerçekleştirilmekte olan işlemlerin kapsamına bağlı saklı yordamları ve Tetikleyicileri kullanma. Uygulamanızı geliştirirken, her işlem istek birim kapasitesi nasıl kullandığı daha iyi anlamak için istek ücret üstbilgisi inceleyin.

## <a name="estimating-throughput-needs"></a>Aktarım hızı gereksinimlerini tahmin etme
Bir istek birimi isteği işleme maliyeti normalleştirilmiş bir ölçüdür. Bir tek istek birimi okumak için gerekli işlem kapasitesi temsil eder (aracılığıyla kendi bağlantı veya kimliği) 10 benzersiz özellik değerlerini (Sistem özellikleri hariç) oluşan tek bir 1 KB'lik öğe. (Ekle) oluşturma isteği değiştirin veya aynı silmek öğe hizmetinden daha fazla işleme kullanır ve böylelikle daha fazla istek birimleri gerektirir. 

> [!NOTE]
> Temel 1 istek birimin bir 1 KB'lik öğe için basit bir kendi bağlantısını GET ya da öğenin kimliği karşılık gelir.
> 
> 

Örneğin, kaç tane istek birimleri sağlamak için üç farklı boyut (1 KB, 4 KB ve 64 KB) ve iki farklı performans düzeylerinde öğeleri gösteren bir tablo İşte (500 Okuma/sn + 100 Yazma/sn ve 500 Okuma/sn + 500 yazma/saniye). Bu örnekte, veri tutarlılığı kümesine **oturumu**, ve dizin oluşturma ilkesini ayarlamak **hiçbiri**.

| Öğe boyutu | Okuma/saniye | Yazma/saniye | İstek birimleri
| --- | --- | --- | --- |
| 1 KB | 500 | 100 | (500 * 1) + (100 * 5) 1.000 RU/sn =
| 1 KB | 500 | 500 | (500 * 1) + (500 * 5) 3.000 RU/sn =
| 4 KB | 500 | 100 | (500 * 1.3) + (100 * 7) = 1.350 RU/sn
| 4 KB | 500 | 500 | (500 * 1.3) + (500 * 7) 4,150 RU/sn =
| 64 KB | 500 | 100 | (500 * 10) + (100 * 48) = 9,800 RU/s
| 64 KB | 500 | 500 | (500 * 10) + (500 * 48) 29,000 RU/sn =

### <a name="use-the-request-unit-calculator"></a>İstek birimi hesaplayıcıyı kullanın
Aktarım hızı tahminleri ayarlamanıza yardımcı olmak için web tabanlı kullanabileceğiniz [istek birimi hesaplayıcı](https://www.documentdb.com/capacityplanner). Hesaplayıcı tahmininize istek birimi gereksinimleri dahil olmak üzere, normal işlemleri için yardımcı olabilir:

* Öğesi (yazar) oluşturur.
* Öğe Okuma
* Öğesini siler
* Öğesi güncelleştirmeleri

Araç ayrıca, sağladığınız örneği maddeler temel alınarak veri depolama gereksinimlerini tahmin etmek için destek içerir.

Aracı kullanmak için:

1. Bir veya daha fazla temsili öğeleri (örneğin, bir örnek JSON belgesi) karşıya yükleyin.
   
    ![İstek birimi hesaplayıcıya öğeleri karşıya yükle][2]
2. Veri depolama gereksinimleri tahmin etmek için depolamayı beklediğiniz öğeleri (örneğin, belge, satır veya köşe) toplam sayısı girin.
3. Oluşturma, okuma, güncelleştirme ve ihtiyaç duyduğunuz silme işlemlerinin sayısı (bir saniye başına temelinde) girin. İstek birimi ücreti öğesi güncelleştirme işlemlerinin tahmin etmek için tipik alan güncelleştirmeler içeren bir adım 1'deki örnek öğenin bir kopyasını yükleyin. Örneğin, madde, güncelleştirmeleri genellikle adlı iki özelliklerini değiştirmek *lastLogin* ve *userVisits*, örnek öğesini kopyalama, bu iki özellik değerlerini güncelleştirin ve sonra kopyalanan öğesi karşıya yükleyin.
   
    ![İstek birimi hesap makinesinde aktarım hızı gereksinimleri girin][3]
4. Seçin **Calculate**ve sonuçları inceleyin.
   
    ![İstek birimi hesaplayıcı sonuçları][4]

> [!NOTE]
> Boyutu ve dizini oluşturulmuş özellik sayısı bakımından önemli ölçüde farklı öğesi türleriniz varsa, her bir örnek yükleyin *türü* tipik öğesi için araç ve ardından sonuçları hesaplar.
> 
> 

### <a name="use-the-azure-cosmos-db-request-charge-response-header"></a>Azure Cosmos DB isteği ücret yanıt üst bilgisi kullanın
Her Azure Cosmos DB hizmetinden gelen yanıt, özel bir başlık içerir (`x-ms-request-charge`) belirli bir istek için kullanılan istek birimleri içerir. Bu üst bilginin Azure Cosmos DB SDK'lar da erişebilirsiniz. .NET SDK'sındaki **RequestCharge** parçasıdır **ResourceResponse** nesne. Sorgular için Azure portalında Azure Cosmos DB Veri Gezgini yürütülen sorgular için ücret bilgi sağlar. Nasıl alınacağı ve aktarım hızı ayarlama hakkında kullanarak farklı çok modelli API'leri öğrenmek için [ayarlayın ve Azure Cosmos DB'de aktarım hızı alma](set-throughput.md) makalesi.

Ayrılmış aktarım hızı uygulamanız için gereken miktarı tahmin etmek için bir yöntem, uygulamanız tarafından kullanılan bir temsili öğesi karşı tipik işlemlerin çalıştırmayla ilgili istek birimi ücreti kaydetmektir. Ardından, her saniye gerçekleştirmek için tahmin işlemlerin sayısını tahmin edin. Ayrıca ölçün ve tipik sorgular ve Azure Cosmos DB betik kullanımı dahil emin olun.

> [!NOTE]
> Boyutu ve dizini oluşturulmuş özellik sayısı bakımından önemli ölçüde farklı öğesi türleriniz varsa, her ilişkilendirilmiş geçerli işlemi istek birimi ücreti kayıt *türü* tipik öğesi.
> 
> 

Örneğin, adımlar şunlardır:

1. Kayıt oluşturma (eklerken) istek birimi ücreti tipik bir öğe. 
2. Tipik bir öğe Okuma istek birimi ücreti kaydedin.
3. Tipik bir öğe güncelleştirme istek birimi ücreti kaydedin.
4. İstek birimi ücretine ek olarak normal, sık kullanılan öğesi sorgularının kaydedin.
5. İstek birimi ücretine ek olarak uygulamanın kullandığı tüm özel komut dosyaları (saklı yordamlar, Tetikleyiciler, kullanıcı tanımlı işlevler) kaydedin.
6. Tahmini saniyede çalıştırılacak tahmin işlemlerin sayısı belirtilen gerekli istek birimleri hesaplayın.

## <a name="a-request-unit-estimate-example"></a>Bir istek birimi tahmini örnek
Yaklaşık 1 KB boyutundaki aşağıdaki belge göz önünde bulundurun:

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
> Belgeler, yukarıdaki belgenin sistem tarafından hesaplanan boyutu 1 KB'lık biraz daha küçüktür, bu nedenle Azure Cosmos DB'de küçültülmüş.
> 
> 

Aşağıdaki tabloda, bu öğe üzerinde normal işlemleri için yaklaşık istek birimi ücreti gösterir. (Yaklaşık istek birimi ücretine ek olarak, hesap tutarlılık düzeyi ayarlamak varsayar **oturumu** ve tüm öğeleri otomatik olarak dizine alınmış.)

| İşlem | İstek birimi ücreti |
| --- | --- |
| Öğe oluştur |~15 RU |
| Öğe Okuma |~ 1 RU |
| Sorgu öğesi Kimliğine göre |~2.5 RU |

Aşağıdaki tabloda, yaklaşık istek birimi ücreti uygulamada kullanılan tipik sorgular için gösterilir:

| Sorgu | İstek birimi ücreti | döndürülen öğe sayısı |
| --- | --- | --- |
| Gıda Kimliğine göre seçin |~2.5 RU |1 |
| Üreticiye göre foods seçin |~7 RU |7 |
| Gıda grubu ve sipariş ağırlığa göre seçin |~70 RU |100 |
| İlk 10 foods bir yiyecek grubunda seçin |~ 10 RU |10 |

> [!NOTE]
> İstek birimi, geri döndürülen öğe sayısına göre değişiklik gösterir.
> 
> 

Bu bilgilerle, işlemler ve sorgular beklediğiniz saniye başına sayısı verilen bu uygulama için istek birimi gereksinimlerini tahmin edebilirsiniz:

| İşlem/sorgu | Saniye başına tahmini sayısı | Gerekli istek birimleri |
| --- | --- | --- |
| Öğe oluştur |10 |150 |
| Öğe Okuma |100 |100 |
| Üreticiye göre foods seçin |25 |175 |
| Gıda gruplandırma ölçütü seçin |10 |700 |
| İlk 10 seçin |15 |150 toplam |

Bu durumda, bir ortalama aktarım hızı gereksinimi 1,275 RU/saniye bekler. Yuvarlama en yakın 100 adede kadar RU/saniye için bu uygulamanın kapsayıcı (veya kapsayıcıları kümesi) 1,300 sağlamanız.

## <a id="RequestRateTooLarge"></a> Azure Cosmos DB'de ayrılmış aktarım hızı sınırlarını aşma
İstek birimi tüketimini, saniye başına fiyatı üzerinden değerlendirilir. Hızı sağlanan aktarım hızı düzeyinin altına düşene kadar sağlanan istek birimi hızı aşan uygulamaları için istek oranı sınırlı. İstek oranı sınırlı olduğunda, sunucu sıd'lerde istekle sona `RequestRateTooLargeException` (HTTP durum kodu 429) ve döndürür `x-ms-retry-after-ms` başlığı. Üst bilgisi, kullanıcı isteği yeniden denemeden önce beklemesi gereken milisaniye cinsinden süre miktarını gösterir.

    HTTP Status 429
    Status Line: RequestRateTooLarge
    x-ms-retry-after-ms :100

.NET İstemci SDK'sı ve LINQ sorguları kullanırsanız, çoğu zaman, hiçbir zaman bu özel durum ile uğraşmak zorunda. .NET İstemci SDK'ın geçerli sürümü bu yanıt örtük olarak yakalayan, sunucu tarafından belirtilen retry-after üst bilgisi uyar ve istek otomatik olarak yeniden dener. Hesabınızda aynı anda birden çok istemci tarafından erişilen sürece sonraki yeniden deneme işlemi başarılı olur.

İstek hızı üst üste çalışan birden fazla istemci varsa, varsayılan yeniden deneme davranışı yeterli olmayabilir ve istemci oluşturur bir `DocumentClientException` 429 uygulama durumuyla kod. Bu gibi durumlarda, yeniden deneme davranışı ve uygulamanızın hata işleme rutinleri mantığının işleme düşünün veya kapsayıcı (veya kapsayıcıları kümesi) için sağlanan aktarım hızı artırmak isteyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
 
- Bilgi edinmek için nasıl [ayarlayın ve Azure Cosmos DB'de aktarım hızı alma](set-throughput.md) Azure portal ve SDK'ları kullanarak.
- Hakkında bilgi edinin [performans ve ölçek testi Azure Cosmos DB ile](performance-testing.md).
- Ayrılmış işleme ile Azure Cosmos DB veritabanları hakkında daha fazla bilgi için bkz: [Azure Cosmos DB fiyatlandırma](https://azure.microsoft.com/pricing/details/cosmos-db/) ve [verileri Azure Cosmos DB'de bölümleme](partition-data.md).
- Azure Cosmos DB hakkında daha fazla bilgi için bkz: [Azure Cosmos DB belgelerini](https://azure.microsoft.com/documentation/services/cosmos-db/). 

[2]: ./media/request-units/RUEstimatorUpload.png
[3]: ./media/request-units/RUEstimatorDocuments.png
[4]: ./media/request-units/RUEstimatorResults.png
[5]: ./media/request-units/RUCalculator2.png


