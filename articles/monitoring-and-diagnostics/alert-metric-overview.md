---
title: Nasıl iş ölçüm uyarıları anlamak Azure İzleyici'de.
description: Ölçüm uyarıları ile neler yapabileceğinizi ve Azure İzleyici'de nasıl çalıştıkları hakkında genel bir bakış edinin.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.component: alerts
ms.openlocfilehash: 1ec47ddf5769dd8ed624277a86db57f449581b90
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46948698"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Nasıl iş ölçüm uyarıları anlamak Azure İzleyici'de

Azure İzleyici ölçüm uyarıları çok boyutlu ölçümler üzerinde çalışır. Bu ölçümler, platform ölçümleri, özel ölçümler (Önizleme) popüler günlüklerinden Log Analytics ölçümlerine, Application Insights standart ölçüm dönüştürülen olabilir. Ölçüm uyarıları olmadığını denetlemek için düzenli aralıklarla değerlendirin koşulları bir ya da ölçüm, zaman serisi doğruysa ve değerlendirmeleri karşılandığında size bildirir. Ölçüm uyarıları diğer bir deyişle, durumu değiştiğinde yalnızca bildirimleri gönderdikleri bilgisi yok.

## <a name="how-do-metric-alerts-work"></a>Ölçüm uyarıları nasıl çalışır

İzlenen ölçüm adı ve koşul (bir işleç ve bir eşik) ve uyarı kuralı tetiklendiğinde tetiklenmesi için bir eylem grubu için bir hedef kaynak belirterek, bir ölçüm uyarısı kuralının tanımlayabilirsiniz.
Basit bir ölçüm uyarısı kuralının gibi oluşturduğunuz varsayalım:

- Hedef kaynak (izlemek istediğiniz Azure kaynağını): myVM
- Ölçüm: CPU yüzdesi
- Zaman toplama (ham ölçüm değerleri üzerinde çalıştırılan istatistiği. Toplamaları olan Min, Max, Avg, toplam desteklenen): ortalama
- Süre (ölçüm değerleri denetlenir görünüm arka pencere): son 5 dakika boyunca
- Sıklık (ile ölçüm uyarısı denetleyen koşullar karşılandığında sıklık düzeyi): 1 dakika
- İşleci: Büyüktür
- Eşik: 70

Uyarı kuralı oluşturulur zamandan İzleyici her 1 dakikada bir çalışır ve son 5 dakika için ölçüm değerlerinde görünümünü ve bu değerlerin ortalamasını 70 aşıp aşmadığını denetler. Koşul diğer bir deyişle, son 5 dakika için ortalama CPU yüzdesi 70 aşıyor, etkin bir bildirim uyarı kuralı tetikler. Uyarı kuralı ile ilişkili eylem grubundaki bir e-posta veya bir web kancası eylemi yapılandırdıysanız, hem de etkin bir bildirim alırsınız.

Bu uyarı kuralı Açmadığınızda belirli bir örneğini de tüm uyarılar dikey penceresinde Azure portalında görüntülenebilir.

Örneğin, "myVM" kullanım eşiğin üstünde durdurulmasını sonraki denetimlerinde devam eder, koşul çözülene kadar uyarı kuralı yeniden tetiklenmez.

Süre "myVM" kullanım geri normal geliyorsa, diğer bir deyişle, belirtilen eşiğin altında geçtikten sonra. Uyarı kuralı koşulu çözümlenen bildirim göndermek için iki birden fazla kez izler. Koşullar kanatların durumunda gürültüsünü azaltmak art arda üç nokta için Uyarı koşulu karşılanmadı zaman uyarı kuralı çözümlendi/devre dışı bir ileti gönderir.

Çözümlenen bildirimi web kancaları ya da e-posta ile gönderilir gibi Azure portalında uyarı örneği (İzleyici durumu olarak adlandırılır) durumunu da çözümlenen ayarlanır.

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Uygun ölçekte Azure İzleyicisi'nde ölçüm uyarıları kullanarak izleme

### <a name="using-dimensions"></a>Boyutları kullanma

Azure İzleyici ölçüm uyarıları tek bir kural ile birden çok boyut değeri birleşimleri izleme de destekler. Birden çok boyut birleşimleri örneği yardımıyla neden kullanabileceğinize bakalım.

Web siteniz için bir App Service planı olduğunu varsayalım. Web sitesi/uygulama çalıştıran birden fazla CPU kullanımı izlemek istediğiniz. Bir ölçüm uyarısı kuralının gibi kullanarak bunu yapabilirsiniz

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Boyutlar
  - Örnek InstanceName1, InstanceName2 =
- Zaman toplama: ortalama
- Dönem: son 5 dakika
- Sıklık: 1 dakika
- İşleç: GreaterThan
- Eşik: 70

Son 5 dakika için ortalama CPU kullanımı % 70'i aşarsa gibi daha önce bu kuralı izler. Ancak aynı kurala iki örnek, Web sitenizi çalıştırmak izleyebilirsiniz. Her örnek tek tek izlenen ve ayrı ayrı bildirimler alırsınız.

Söyleyin, yoğun talep görmesini bir web uygulamasına sahip ve daha fazla örnek eklenmesi gerekir. Yukarıdaki kuralı, yalnızca iki örnek yine de izler. Ancak, şu şekilde bir kural oluşturabilirsiniz.

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Boyutlar
  - Örneği = *
- Zaman toplama: ortalama
- Dönem: son 5 dakika
- Sıklık: 1 dakika
- İşleç: GreaterThan
- Eşik: 70

Bu kural için örnek yani tüm değerleri otomatik olarak izler Ölçüm uyarı kuralınızı yeniden değiştirmek zorunda kalmadan çıktıkça örneklerinizin izleyebilirsiniz.

### <a name="monitoring-multiple-resource-using-metric-alerts"></a>Birden çok kaynak ölçüm uyarıları kullanarak izleme

Önceki bölümde görüldüğü gibi tek tek boyut birleşimi (yani her izleyen tek bir ölçüm uyarısı kuralının olma olasılığı vardır bir ölçüm zaman serisi). Ancak, aynı anda bir kaynak yapmayı hala sınırını aşmamanız gerekir. Ölçüm uyarıları artık de önizleme bir kuralda birden fazla kaynaklarla izleme desteği sunar. VM'lerin 100'lük Bloklar, aboneliğinizde varsa, bu yeni özellik, hızlı bir şekilde bunlar için izleme işlevini ayarlama yardımcı olur. 

Bu özellik şu anda önizleme sürümündedir. Birden çok kaynak izleme ölçüm uyarı kuralları oluşturma, Azure portalından şu anda desteklenmiyor. Azure Resource Manager şablonları aracılığıyla bu kurallar oluşturabilirsiniz.

## <a name="typical-latency"></a>Tipik bir gecikme süresi

Uyarı kuralı sıklığı 1 dakika olarak ayarlarsanız ölçüm uyarıları için genellikle, 5 dakika içinde bildirimi alırsınız. Bildirim sistemleri için ağır yük durumlarda daha uzun bir gecikme süresi görebilirsiniz.

## <a name="supported-resource-types-for-metric-alerts"></a>Desteklenen kaynak türleri için ölçüm uyarıları

Bu konuda desteklenen kaynak türlerini tam listesini bulabilirsiniz [makale](monitoring-near-real-time-metric-alerts.md#metrics-and-dimensions-supported)

## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturun, görüntüleyin ve azure'da ölçüm Uyarıları yönetme hakkında bilgi edinin](alert-metric.md)
- [Ölçüm uyarıları Azure Resource Manager şablonlarını kullanarak dağıtma hakkında bilgi edinin](monitoring-create-metric-alerts-with-templates.md)
- [Eylem grupları hakkında daha fazla bilgi edinin](monitoring-action-groups.md)
