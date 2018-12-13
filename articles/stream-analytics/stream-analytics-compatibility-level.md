---
title: Azure Stream Analytics işleri için uyumluluk düzeyini anlayın
description: Son uyumluluk düzeyinde bir Azure Stream Analytics işi ve önemli değişiklikler için uyumluluk düzeyi hakkında bilgi edinin
services: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: b0e0f26abbf8eb5cbf1cf9ba2014204d773ae15d
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2018
ms.locfileid: "53187322"
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için uyumluluk düzeyi
 
Uyumluluk düzeyi için bir Azure Stream Analytics hizmetinin sürüm özgü davranışları gösterir. Azure Stream Analytics normal özellik güncelleştirmeleri ve performans geliştirmeleri ile yönetilen bir hizmet. Genellikle güncelleştirmeleri otomatik olarak son kullanıcılar için kullanılabilir hale getirilir. Ancak, bazı yeni özellikler tür olarak değişiklik var olan bir işi davranışındaki ana değişikliğine neden, bu işleri gibi veri kullanan işlemleri değiştirin. Bir uyumluluk düzeyi, Stream Analytics'te sunulan büyük bir değişiklik temsil etmek için kullanılır. Önemli değişiklikler her zaman yeni bir uyumluluk düzeyi ile kullanıma sunulmuştur. 

Uyumluluk düzeyi mevcut işleri herhangi bir hata çalıştırmak emin olur. Yeni bir Stream Analytics işi oluşturduğunuzda, sizin için kullanılabilir olan en son uyumluluk düzeyini kullanarak oluşturmak için en iyi bir uygulamadır. 
 
## <a name="set-a-compatibility-level"></a>Bir uyumluluk düzeyi ayarlayın 

Uyumluluk düzeyi, bir stream analytics işi çalışma zamanı davranışını denetler. Portalı kullanarak veya kullanarak, bir Stream Analytics işine ilişkin uyumluluk düzeyini ayarlayabilirsiniz [oluşturma işi REST API çağrısı](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics şu anda iki Uyumluluk Düzeyleri-"1.0" ve "1.1" destekler. Varsayılan olarak, "Azure Stream Analytics genel kullanıma sunulduğunda sunulan 1.0" uyumluluk düzeyi ayarlanır. Varsayılan değer güncelleştirmek için var olan Stream Analytics işinize gidin > seçin **uyumluluk düzeyi** seçeneğini **yapılandırma** bölümünde ve değeri değiştirin. 

Uyumluluk düzeyini güncelleştirmeden önce işi durdurmanız emin olun. İşinizi çalışır durumda olup olmadığını uyumluluk düzeyini güncelleştirilemiyor. 

![Azure portalında Stream Analytics uyumluluk düzeyi](media/stream-analytics-compatibility-level/stream-analytics-compatibility.png)

 
Uyumluluk düzeyini güncelleştirdiğinizde, T-SQL derleyicisi iş seçili uyumluluk düzeyine karşılık gelen söz dizimi ile doğrular. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Önemli değişikliklere en son uyumluluk düzeyini (1.1)

Uyumluluk düzeyi 1.1 aşağıdaki önemli değişiklikler yapılmıştır:

* **Service Bus XML biçimi**  

  * **Önceki sürümler:** İleti içeriğini XML etiketleri içerdiği için DataContractSerializer, azure Stream Analytics kullanılır. Örneğin:
    
    @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ "SensorId": "1", "Sıcaklık": 64\}\u0001 

  * **Geçerli sürüm:** İleti içeriğini hiçbir ek etiketler ile doğrudan bir akış içeriyor. Örneğin:
  
    {"SensorId": "1", "Sıcaklık": 64} 
 
* **Alan adları için kalıcı büyük küçük harf duyarlılığı**  

  * **Önceki sürümler:** Azure Stream Analytics altyapısı tarafından işlendiğinde küçük harfe alan adları değiştirildi. 

  * **Geçerli sürüm:** Azure Stream Analytics altyapısı tarafından işlendiğinde alan adları için büyük küçük harf duyarlılığı kalıcıdır. 

    > [!NOTE] 
    > Kalıcı büyük küçük harf duyarlılığı Edge ortamı kullanarak barındırılan bir Stream Analytic işleri için henüz desteklenmiyor. Sonuç olarak, işinizin Edge üzerinde barındırılıyorsa tüm alan adlarının küçük harfe dönüştürülür. 

* **FloatNaNDeserializationDisabled**  

  * **Önceki sürümler:** CREATE TABLE komut olaylarla NaN (bir sayı değil. filtre değil Örneğin, sonsuz, - sonsuz) kayan noktalı sayı sütunundaki bu numaraları için belgelenmiş aralık dışında olduğundan yazın.

  * **Geçerli sürüm:** CREATE TABLE güçlü bir şema belirtmenizi sağlar. Stream Analytics altyapısı, verileri bu şemaya uygun olduğunu doğrular. Bu modelde, komut NaN değerleri ile olayları filtreleyebilirsiniz. 

* **Otomatik yukarı çevrim JSON dizeleri datetime için devre dışı bırakın.**  

  * **Önceki sürümler:** JSON ayrıştırıcının DateTime türü tarih/saat/dilimi bilgileri ile otomatik olarak başvurmanıza dize değerleri olur ve ardından UTC'ye dönüştürün. Bu, saat dilimi bilgilerini kesilmesine sonuçlandı.

  * **Geçerli sürüm:** Artık otomatik olarak başvurmanıza DateTime türü tarih/saat/dilimi bilgileri ile dize değerlerinin yoktur. Sonuç olarak, saat dilimi bilgilerini tutulur. 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics girişleri sorunlarını giderme](stream-analytics-troubleshoot-input.md)
* [Stream Analytics kaynak durumu dikey penceresi](stream-analytics-resource-health.md)
