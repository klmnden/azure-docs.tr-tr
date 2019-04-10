---
title: Azure Stream Analytics işleri için uyumluluk düzeyini anlayın
description: Son uyumluluk düzeyinde bir Azure Stream Analytics işi ve önemli değişiklikler için uyumluluk düzeyi hakkında bilgi edinin
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/08/2019
ms.openlocfilehash: 6fb93152263d253de983b17d25f02f4c68a172fd
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2019
ms.locfileid: "59361395"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için uyumluluk düzeyi
 
Uyumluluk düzeyi için bir Azure Stream Analytics hizmetinin sürüm özgü davranışları gösterir. Azure Stream Analytics normal özellik güncelleştirmeleri ve performans geliştirmeleri ile yönetilen bir hizmet. Genellikle güncelleştirmeleri otomatik olarak son kullanıcılar için kullanılabilir hale getirilir. Ancak, bazı yeni özellikler tür olarak değişiklik var olan bir işi davranışındaki ana değişikliğine neden, bu işleri gibi veri kullanan işlemleri değiştirin. Bir uyumluluk düzeyi, Stream Analytics'te sunulan büyük bir değişiklik temsil etmek için kullanılır. Önemli değişiklikler her zaman yeni bir uyumluluk düzeyi ile kullanıma sunulmuştur. 

Uyumluluk düzeyi, herhangi bir hata mevcut işlerinizi emin olur. Yeni bir Stream Analytics işi oluşturduğunuzda, en son uyumluluk düzeyini kullanarak oluşturmak için en iyi bir uygulamadır. 
 
## <a name="set-a-compatibility-level"></a>Bir uyumluluk düzeyi ayarlayın 

Uyumluluk düzeyi, bir stream analytics işi çalışma zamanı davranışını denetler. Portalı kullanarak veya kullanarak, bir Stream Analytics işine ilişkin uyumluluk düzeyini ayarlayabilirsiniz [oluşturma işi REST API çağrısı](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics şu anda iki Uyumluluk Düzeyleri-"1.0" ve "1.1" destekler. Varsayılan olarak, "Azure Stream Analytics genel kullanıma sunulduğunda sunulan 1.0" uyumluluk düzeyi ayarlanır. Varsayılan değer güncelleştirmek için var olan Stream Analytics işinize gidin > seçin **uyumluluk düzeyi** seçeneğini **yapılandırma** bölümünde ve değeri değiştirin. 

Uyumluluk düzeyini güncelleştirmeden önce işi durdurmanız emin olun. İşinizi çalışır durumda olup olmadığını uyumluluk düzeyini güncelleştirilemiyor. 

![Azure portalında Stream Analytics uyumluluk düzeyi](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

 
Uyumluluk düzeyini güncelleştirdiğinizde, T-SQL derleyicisi iş seçili uyumluluk düzeyine karşılık gelen söz dizimi ile doğrular. 

## <a name="major-changes-in-the-latest-compatibility-level-12"></a>Önemli değişikliklere en son uyumluluk düzeyini (1.2)

Uyumluluk düzeyi 1.2 aşağıdaki önemli değişiklikler yapılmıştır:

### <a name="geospatial-functions"></a>Jeo-uzamsal işlevler 

**Önceki sürümler:** Azure Stream Analytics, Geography hesaplamalar kullanılır.

**Geçerli sürüm:** Azure Stream Analytics, geometrik öngörülen coğrafi koordinatları işlem olanak tanır. Jeo-uzamsal işlevler imzası bir değişiklik yoktur. Ancak, kendi semantiği daha önce daha kesin hesaplama izin vererek biraz farklı olacaktır.

Azure Stream Analytics, Jeo-uzamsal başvuru veri dizin oluşturmayı destekler. Daha hızlı bir birleşim hesaplama için Jeo-uzamsal öğeleri içeren reference veri sıralanabilir.

Güncelleştirilmiş Jeo-uzamsal İşlevler, iyi bilinen metin (WKT) Jeo-uzamsal biçimi tam anlamlılık getirin. Daha önce desteklenen olmayan diğer Jeo-uzamsal bileşenleri ile GeoJson belirtebilirsiniz.

Daha fazla bilgi için [Azure Stream Analytics – Bulut ve IOT Edge Jeo-uzamsal özellikler güncelleştirmeleri](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Giriş kaynakları ile birden çok bölümü paralel sorgu yürütme 

**Önceki sürümler:** Azure Stream Analytics sorguları sorgu işleme giriş kaynağı bölümler arasında paralel hale getirmek için PARTITION BY yan tümcesinin kullanımı gereklidir.

**Geçerli sürüm:** Sorgu mantığının giriş kaynağı bölümler arasında paralel hale, Azure Stream Analytics sorgu ayrı örneklerini oluşturur ve hesaplamalar paralel olarak çalıştırır.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>CosmosDB çıkışı ile yerel toplu API tümleştirmesi

**Önceki sürümler:** Upsert davranış olduğu *eklemek veya birleştirme*.

**Geçerli sürüm:** CosmosDB çıkışı yerel toplu API tümleştirmesiyle, aktarım hızını en üst düzeye çıkarır ve istekleri azaltma verimli bir biçimde gerçekleştirir.

Upsert davranıştır *Ekle veya Değiştir*.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>SQL çıkışını yazarken DateTimeOffset

**Önceki sürümler:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) türleri UTC'ye ayarlanır.

**Geçerli sürüm:** DateTimeOffset artık ayarlanır.

### <a name="strict-validation-of-prefix-of-functions"></a>Katı doğrulama önekin işlevleri

**Önceki sürümler:** Katı doğrulama işlevi öneklerinin vardı.

**Geçerli sürüm:** Azure Stream Analytics, katı bir doğrulama işlevi önekleri vardır. Yerleşik bir işleve bir ön ek ekleme, bir hataya neden olur. Örneğin,`myprefix.ABS(…)` desteklenmiyor.

Yerleşik Toplamalar için bir önek ekleme hatasına neden olur. Örneğin, `myprefix.SUM(…)` desteklenmiyor.

Tüm kullanıcı tanımlı işlevleri sonuçları hata için "Sistem" öneki kullanıyor.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>Dizi ve nesne Cosmos DB çıkış bağdaştırıcısı anahtar özellikleri olarak izin verme

**Önceki sürümler:** Dizi ve nesne türleri, bir anahtar özellik olarak desteklendi.

**Geçerli sürüm:** Dizi ve nesne türleri, artık bir anahtar özellik olarak desteklenir.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics girişleri sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Stream Analytics kaynak durumu](stream-analytics-resource-health.md)
