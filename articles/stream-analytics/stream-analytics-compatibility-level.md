---
title: Azure Stream Analytics işleri için uyumluluk düzeyini anlayın
description: Son uyumluluk düzeyinde bir Azure Stream Analytics işi ve önemli değişiklikler için uyumluluk düzeyi hakkında bilgi edinin
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 04/12/2019
ms.openlocfilehash: b5c833798f8533e7c6fbe3595a726ac6ce56e2d2
ms.sourcegitcommit: c3d1aa5a1d922c172654b50a6a5c8b2a6c71aa91
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59682823"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için uyumluluk düzeyi

Bu makalede, Azure Stream Analytics uyumluluk düzeyi seçeneğinde açıklanır. Stream Analytics, normal özellik güncelleştirmeleri ve performans geliştirmeleri ile yönetilen bir hizmet olan. Hizmetin çalışma zamanları güncelleştirmelerin çoğu, otomatik olarak son kullanıcıların kullanımına sunulur. 

Ancak, bazı yeni işlevler hizmetinde var olan bir işi davranışını bir değişiklik gibi büyük bir değişiklik neden olabilir veya çalışan iş şekilde bir değişiklik tüketilen. Azaltılmış ayarı uyumluluk düzeyini bırakarak önemli değişiklikler çalışan mevcut, Stream Analytics işleri tutabilirsiniz. En son çalışma zamanı davranışları için hazır olduğunuzda, uyumluluk düzeyini yükselterek katılımı. 

## <a name="choose-a-compatibility-level"></a>Bir uyumluluk düzeyini seçin

Uyumluluk düzeyi, bir stream analytics işi çalışma zamanı davranışını denetler. 

Azure Stream Analytics, şu anda üç uyumluluk düzeylerini destekler:

* 1.0 - varsayılan düzeyi
* 1.1 - geçerli yayın davranışı
* 1.2 (Önizleme) - en yeni davranışı ile değerlendirme en son yenilikleri

Özgün 1.0 uyumluluk düzeyi'nin genel kullanılabilirliğinin Azure Stream Analytics sırasında birkaç yıl önce kullanıma sunulmuştur.

Yeni bir Stream Analytics işi oluşturduğunuzda, en son uyumluluk düzeyini kullanarak oluşturmak için en iyi bir uygulamadır. Daha sonra eklenen değişiklik ve karmaşıklık önlemek için en son davranışlar üzerinde bağlı olan iş tasarımı başlatın.

## <a name="set-the-compatibility-level"></a>Uyumluluk düzeyini ayarlayın

Kullanarak veya Azure portalında bir Stream Analytics işine ilişkin uyumluluk düzeyini ayarlayabilirsiniz [oluşturma işi REST API çağrısı](/rest/api/streamanalytics/stream-analytics-job).

Azure portalında iş uyumluluk düzeyini güncelleştirmek için:

1. Kullanım [Azure portalında](https://portal.azure.com) Stream Analytics işinizi bulunacak.
2. **Durdur** uyumluluk düzeyini güncelleştirmeden önce iş. İşinizi çalışır durumda olup olmadığını uyumluluk düzeyini güncelleştirilemiyor.
3. Altında **yapılandırma** başlığı seçin **uyumluluk düzeyi**.
4. İstediğiniz uyumluluk düzeyi değeri seçin.
5. Seçin **Kaydet** sayfanın alt kısmındaki.

![Azure portalında Stream Analytics uyumluluk düzeyi](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

Uyumluluk düzeyini güncelleştirdiğinizde, T-SQL derleyicisi iş seçili uyumluluk düzeyine karşılık gelen söz dizimi ile doğrular.

## <a name="compatibility-level-12"></a>Uyumluluk düzeyi 1.2

Uyumluluk düzeyi 1.2 aşağıdaki önemli değişiklikler yapılmıştır:

### <a name="geospatial-functions"></a>Jeo-uzamsal işlevler

**Önceki düzeyleri:** Azure Stream Analytics, Geography hesaplamalar kullanılır.

**1.2 düzeyi:** Azure Stream Analytics, geometrik öngörülen coğrafi koordinatları işlem olanak tanır. Jeo-uzamsal işlevler imzası bir değişiklik yoktur. Ancak, kendi semantiği daha önce daha kesin hesaplama izin vererek biraz farklı olacaktır.

Azure Stream Analytics, Jeo-uzamsal başvuru veri dizin oluşturmayı destekler. Daha hızlı bir birleşim hesaplama için Jeo-uzamsal öğeleri içeren reference veri sıralanabilir.

Güncelleştirilmiş Jeo-uzamsal İşlevler, iyi bilinen metin (WKT) Jeo-uzamsal biçimi tam anlamlılık getirin. Daha önce desteklenen olmayan diğer Jeo-uzamsal bileşenleri ile GeoJson belirtebilirsiniz.

Daha fazla bilgi için [Azure Stream Analytics – Bulut ve IOT Edge Jeo-uzamsal özellikler güncelleştirmeleri](https://azure.microsoft.com/blog/updates-to-geospatial-functions-in-azure-stream-analytics-cloud-and-iot-edge/).

### <a name="parallel-query-execution-for-input-sources-with-multiple-partitions"></a>Giriş kaynakları ile birden çok bölümü paralel sorgu yürütme

**Önceki düzeyleri:** Azure Stream Analytics sorguları sorgu işleme giriş kaynağı bölümler arasında paralel hale getirmek için PARTITION BY yan tümcesinin kullanımı gereklidir.

**1.2 düzeyi:** Sorgu mantığının giriş kaynağı bölümler arasında paralel hale, Azure Stream Analytics sorgu ayrı örneklerini oluşturur ve hesaplamalar paralel olarak çalıştırır.

### <a name="native-bulk-api-integration-with-cosmosdb-output"></a>CosmosDB çıkışı ile yerel toplu API tümleştirmesi

**Önceki düzeyleri:** Upsert davranış olduğu *eklemek veya birleştirme*.

**1.2 düzeyi:** CosmosDB çıkışı yerel toplu API tümleştirmesiyle, aktarım hızını en üst düzeye çıkarır ve istekleri azaltma verimli bir biçimde gerçekleştirir.

Upsert davranıştır *Ekle veya Değiştir*.

### <a name="datetimeoffset-when-writing-to-sql-output"></a>SQL çıkışını yazarken DateTimeOffset

**Önceki düzeyleri:** [DateTimeOffset](https://docs.microsoft.com/sql/t-sql/data-types/datetimeoffset-transact-sql?view=sql-server-2017) türleri UTC'ye ayarlanır.

**1.2 düzeyi:** DateTimeOffset artık ayarlanır.

### <a name="strict-validation-of-prefix-of-functions"></a>Katı doğrulama önekin işlevleri

**Önceki düzeyleri:** Katı doğrulama işlevi öneklerinin vardı.

**1.2 düzeyi:** Azure Stream Analytics, katı bir doğrulama işlevi önekleri vardır. Yerleşik bir işleve bir ön ek ekleme, bir hataya neden olur. Örneğin,`myprefix.ABS(…)` desteklenmiyor.

Yerleşik Toplamalar için bir önek ekleme hatasına neden olur. Örneğin, `myprefix.SUM(…)` desteklenmiyor.

Tüm kullanıcı tanımlı işlevleri sonuçları hata için "Sistem" öneki kullanıyor.

### <a name="disallow-array-and-object-as-key-properties-in-cosmos-db-output-adapter"></a>Dizi ve nesne Cosmos DB çıkış bağdaştırıcısı anahtar özellikleri olarak izin verme

**Önceki düzeyleri:** Dizi ve nesne türleri, bir anahtar özellik olarak desteklendi.

**1.2 düzeyi:** Dizi ve nesne türleri, artık bir anahtar özellik olarak desteklenir.

## <a name="compatibility-level-11"></a>Uyumluluk düzeyi 1.1

Uyumluluk düzeyi 1.1 aşağıdaki önemli değişiklikler yapılmıştır:

### <a name="service-bus-xml-format"></a>Service Bus XML biçimi

**1.0 düzeyi:** İleti içeriğini XML etiketleri içerdiği için DataContractSerializer, azure Stream Analytics kullanılır. Örneğin:

`@\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId":"1", "Temperature":64\}\u0001`

**1.1 düzeyi:** İleti içeriğini hiçbir ek etiketler ile doğrudan bir akış içeriyor. Örneğin, `{ "SensorId":"1", "Temperature":64}`

### <a name="persisting-case-sensitivity-for-field-names"></a>Alan adları için kalıcı büyük küçük harf duyarlılığı

**1.0 düzeyi:** Azure Stream Analytics altyapısı tarafından işlendiğinde küçük harfe alan adları değiştirildi.

**1.1 düzeyi:** Azure Stream Analytics altyapısı tarafından işlendiğinde alan adları için büyük küçük harf duyarlılığı kalıcıdır.

> [!NOTE]
> Kalıcı büyük küçük harf duyarlılığı Edge ortamı kullanarak barındırılan bir Stream Analytic işleri için henüz desteklenmiyor. Sonuç olarak, işinizin Edge üzerinde barındırılıyorsa tüm alan adlarının küçük harfe dönüştürülür.

### <a name="floatnandeserializationdisabled"></a>FloatNaNDeserializationDisabled

**1.0 düzeyi:** CREATE TABLE komut olaylarla NaN (bir sayı değil. filtre değil Örneğin, sonsuz, - sonsuz) kayan noktalı sayı sütunundaki bu numaraları için belgelenmiş aralık dışında olduğundan yazın.

**1.1 düzeyi:** CREATE TABLE güçlü bir şema belirtmenizi sağlar. Stream Analytics altyapısı, verileri bu şemaya uygun olduğunu doğrular. Bu modelde, komut NaN değerleri ile olayları filtreleyebilirsiniz.

### <a name="disable-automatic-upcast-for-datetime-strings-in-json"></a>Otomatik yukarı çevrim JSON dizeleri datetime için devre dışı bırak

**1.0 düzeyi:** JSON ayrıştırıcının DateTime türü tarih/saat/dilimi bilgileri ile otomatik olarak başvurmanıza dize değerleri olur ve ardından UTC'ye dönüştürün. Bu davranış, saat dilimi bilgilerini kesilmesine sonuçlandı.

**1.1 düzeyi:** Artık otomatik olarak başvurmanıza DateTime türü tarih/saat/dilimi bilgileri ile dize değerlerinin yoktur. Sonuç olarak, saat dilimi bilgilerini tutulur.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Stream Analytics girişleri sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Stream Analytics kaynak durumu](stream-analytics-resource-health.md)
