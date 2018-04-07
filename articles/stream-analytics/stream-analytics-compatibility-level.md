---
title: Azure Stream Analytics işleri için uyumluluk düzeyi anlama
description: Son uyumluluk düzeyi bir Azure akış analizi işi ve önemli değişiklikler için uyumluluk düzeyi öğrenin
services: stream-analytics
author: jasonwhowell
ms.author: jasonh
manager: kfile
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 01/03/2018
ms.openlocfilehash: 32e73918b2dd98822d42d74002b705ff730145d9
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2018
---
# <a name="compatibility-level-for-azure-stream-analytics-jobs"></a>Azure Stream Analytics işleri için uyumluluk düzeyi
 
Bir Azure akış analizi hizmeti sürüm özgü davranışlarını uyumluluk düzeyini gösterir. Azure Stream Analytics normal özelliği güncelleştirmeler ve performans geliştirmeleri ile yönetilen bir hizmet var. Genellikle güncelleştirmeleri otomatik olarak son kullanıcılar için kullanılabilir hale getirilir. Ancak, bazı yeni özellikler gibi olarak-davranış değişikliği mevcut bir işin önemli değişiklikler tanıtmak, bu işleri gibi verileri tüketen işlemlerde değiştirin. Bir uyumluluk düzeyi, Stream Analytics içinde sunulan önemli bir değişiklik temsil etmek için kullanılır. Önemli değişiklikler her zaman yeni bir uyumluluk düzeyi ile sunulur. 

Uyumluluk düzeyi varolan işlerini herhangi bir hata çalıştırmak emin olur. Yeni bir akış analizi işi oluşturduğunuzda, sizin için kullanılabilir olan en son uyumluluk düzeyini kullanarak oluşturmak için en iyi bir uygulamadır. 
 
## <a name="set-a-compatibility-level"></a>Bir uyumluluk düzeyini ayarlayın 

Uyumluluk düzeyi akış analizi işi çalışma zamanı davranışını denetler. Portalı kullanarak veya kullanarak Stream Analytics işi için uyumluluk düzeyi ayarlayabilirsiniz [oluşturma işi REST API çağrısı](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-job). Azure Stream Analytics şu anda iki Uyumluluk Düzeyleri-"1.0" ve "1.1" destekler. Varsayılan olarak, uyumluluk düzeyi ", Azure akış analizi genel kullanılabilirlik sırasında tanıtılan 1.0" ayarlanır. Varsayılan değer güncelleştirmek için var olan Stream Analytics işiniz gidin > seçin **uyumluluk düzeyini** seçeneğini **yapılandırma** bölümünde ve değeri değiştirin. 

Uyumluluk düzeyini güncelleştirmeden önce işini durdurma emin olun. İşinizi çalışır durumda ise uyumluluk düzeyini güncelleştirilemiyor. 

![Uyumluluk düzeyi portalında](media\stream-analytics-compatibility-level/image1.png)

 
Uyumluluk düzeyini güncelleştirdiğinizde, T-SQL derleyici iş seçilen uyumluluk düzeyine karşılık gelen sözdizimi ile doğrular. 

## <a name="major-changes-in-the-latest-compatibility-level-11"></a>Önemli değişikliklere son uyumluluk düzeyini (1.1)

Uyumluluk düzeyi 1.1 aşağıdaki önemli değişiklikler yapılmıştır:

* **Hizmet veri yolu XML biçimi**  

  * **Önceki sürümler:** Azure akış analizi DataContractSerializer, ileti içeriği XML etiketleri dahil şekilde kullanılır. Örneğin:
    
   @\u0006string\b3http://schemas.microsoft.com/2003/10/Serialization/\u0001{ “SensorId”:”1”, “Temperature”:64\}\u0001 

  * **Geçerli sürüm:** ileti içeriği ek etiket ile doğrudan bir akış içeriyor. Örneğin:
  
   { “SensorId”:”1”, “Temperature”:64} 
 
* **Kalıcı büyük küçük harf duyarlılığı bir alan adları**  

  * **Önceki sürümler:** alan adları için Azure Stream Analytics altyapısı tarafından işlendiğinde küçük harflere değiştirildi. 

  * **Geçerli sürüm:** Azure akış analizi altyapısı tarafından işlendiğinde alan adları için büyük küçük harf duyarlılığı kalıcıdır. 

  > [!NOTE] 
  > Kalıcı duyarlılık henüz Edge ortamı kullanarak barındırılan akış analitik işleri için kullanılamaz. Sonuç olarak, işinizi kenarına barındırılıyorsa tüm alan adlarının küçük harflere dönüştürülür. 

* **FloatNaNDeserializationDisabled**  

  * **Önceki sürümler:** CREATE TABLE komutu NaN (bir sayı değil olaylarla değil filtre. Örneğin, sonsuzluk, - sonsuz) FLOAT sütunda bu sayılar için belgelenen aralık dışında olduğundan yazın.

  * **Geçerli sürüm:** CREATE TABLE güçlü bir şema belirtmenize olanak verir. Stream Analytics altyapısı, verileri bu şemasıyla uyumlu olduğunu doğrular. Bu modelde, komut olayları NaN değerler ile filtreleyebilirsiniz. 

* **JSON datetime dizelerde için otomatik yukarı çevrim devre dışı bırakın.**  

  * **Önceki sürümler:** JSON ayrıştırıcı tarih/saat/dilimi bilgilerle DateTime değerleri yazın ve ardından UTC'ye dönüştürmek otomatik olarak başvurmanıza dize gerekir. Bu, saat dilimi bilgilerini kaybetmenize sonuçlandı.

  * **Geçerli sürüm:** yoktur artık otomatik olarak başvurmanıza DateTime türüne tarih/saat/dilimi bilgilerle dize değerleri. Sonuç olarak, saat dilimi bilgileri korunur. 

## <a name="next-steps"></a>Sonraki adımlar
* [Azure akış analizi için sorun giderme kılavuzu](stream-analytics-troubleshooting-guide.md)
* [Stream Analytics kaynak sistem durumu dikey](stream-analytics-resource-health.md)