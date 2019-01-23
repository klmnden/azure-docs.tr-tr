---
title: Nasıl iş ölçüm uyarıları anlamak Azure İzleyici'de.
description: Ölçüm uyarıları ile neler yapabileceğinizi ve Azure İzleyici'de nasıl çalıştıkları hakkında genel bir bakış edinin.
author: snehithm
ms.author: snmuvva
ms.date: 9/18/2018
ms.topic: conceptual
ms.service: azure-monitor
ms.subservice: alerts
ms.openlocfilehash: 9689854d9a28debbfbcf908391806fffac6a2006
ms.sourcegitcommit: 9b6492fdcac18aa872ed771192a420d1d9551a33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/22/2019
ms.locfileid: "54450192"
---
# <a name="understand-how-metric-alerts-work-in-azure-monitor"></a>Nasıl iş ölçüm uyarıları anlamak Azure İzleyici'de

Azure İzleyici ölçüm uyarıları çok boyutlu ölçümler üzerinde çalışır. Bu ölçümler platform ölçümleri olabilir [özel ölçümler](../../azure-monitor/platform/metrics-custom-overview.md), [Log Analytics popüler günlüklerinden dönüştürülen ölçümlerini](../../azure-monitor/platform/alerts-metric-logs.md), Application Insights standart ölçümler. Ölçüm uyarıları olmadığını denetlemek için düzenli aralıklarla değerlendirin koşullara göre bir veya daha fazla ölçüm zaman serisi doğruysa ve değerlendirmeleri karşılandığında size bildirir. Ölçüm Uyarıları durum bilgisi olan, durumu değiştiğinde diğer bir deyişle, bunlar yalnızca bildirimleri gönderin.

## <a name="how-do-metric-alerts-work"></a>Ölçüm uyarıları nasıl çalışır?

Bir ölçüm uyarısı kuralının izlenmesi için bir hedef kaynak, ölçüm adı, koşul türü (statik veya dinamik) ve koşul (bir işleç ve bir eşik/duyarlılık) ve uyarı kuralı tetiklendiğinde tetiklenmesi için bir eylem grubu belirterek tanımlayabilirsiniz. Koşul türlerini eşikleri belirlenir şeklinizi etkiler. [Dinamik eşikler koşul türü ve duyarlılık seçenekleri hakkında daha fazla bilgi](alerts-dynamic-thresholds.md).

### <a name="alert-rule-with-static-condition-type"></a>Statik koşul türü ile uyarı kuralı

Bir basit statik eşik ölçüm uyarısı kuralının gibi oluşturduğunuz varsayalım:

- Hedef kaynak (izlemek istediğiniz Azure kaynağını): myVM
- Ölçüm: CPU yüzdesi
- Koşul türü: Statik
- Zaman toplama (ham ölçüm değerleri üzerinde çalıştırılan istatistiği. Toplamalar Min, Max, zaman Ort, toplam desteklenir): Ortalama
- Süre (ölçüm değerleri denetlenir görünüm arka pencere): Son 5 dakika
- Sıklık (ile ölçüm uyarısı denetleyen koşullar karşılandığında sıklık düzeyi): 1 dakika
- İşleç: Büyüktür
- Eşik: 70

Uyarı kuralı oluşturulur zamandan İzleyici her 1 dakikada bir çalışır ve son 5 dakika için ölçüm değerlerinde görünümünü ve bu değerlerin ortalamasını 70 aşıp aşmadığını denetler. Koşul diğer bir deyişle, son 5 dakika için ortalama CPU yüzdesi 70 aşıyor, etkin bir bildirim uyarı kuralı tetikler. Uyarı kuralı ile ilişkili eylem grubundaki bir e-posta veya bir web kancası eylemi yapılandırdıysanız, hem de etkin bir bildirim alırsınız.

### <a name="alert-rule-with-dynamic-condition-type"></a>Dinamik koşul türü ile uyarı kuralı

Basit bir dinamik eşikler ölçüm uyarı kuralı şu şekilde oluşturduğunuz varsayalım:

- Hedef kaynak (izlemek istediğiniz Azure kaynağını): myVM
- Ölçüm: CPU yüzdesi
- Koşul türü: Dinamik
- Zaman toplama (ham ölçüm değerleri üzerinde çalıştırılan istatistiği. Toplamalar Min, Max, zaman Ort, toplam desteklenir): Ortalama
- Süre (ölçüm değerleri denetlenir görünüm arka pencere): Son 5 dakika
- Sıklık (ile ölçüm uyarısı denetleyen koşullar karşılandığında sıklık düzeyi): 1 dakika
- İşleç: Büyüktür
- Duyarlılık: Orta
- Görünüm geri nokta: 4
- İhlal sayısı: 4

Uyarı kuralı oluşturduktan sonra yapmak için yeni verilere dayalı olarak dinamik makine öğrenimi algoritmasının kullanılabilir geçmiş verileri almak, ölçüm serisi davranış deseni en uygun eşiği hesaplamak ve sürekli olarak olacak eşikleri öğrenin daha doğru eşiği.

Uyarı kuralı oluşturulur zamandan İzleyici her 1 dakikada bir çalışır ve 5 dakika dönemlerine gruplandırılmış son 20 dakikası ölçüm değerlerinde arar ve her birinde 4 nokta süre değerlerinin ortalamasını beklenen eşiği aşıp aşmadığını denetler. Koşul diğer bir deyişle, CPU yüzdesi (dört 5 dakika dönemler) son 20 dakika içinde gelen deviated ortalama dört kez beklenen bir davranıştır, etkin bir bildirim uyarı kuralı tetikler. Uyarı kuralı ile ilişkili eylem grubundaki bir e-posta veya bir web kancası eylemi yapılandırdıysanız, hem de etkin bir bildirim alırsınız.

### <a name="view-and-resolution-of-fired-alerts"></a>Görünüm ve çözümü tetiklenen uyarılar

Tetikleme uyarı kuralları yukarıdaki örnekleri de Azure portalında görüntülenebilir **tüm uyarıları** dikey penceresi.

Varsayalım "myVM" kullanımı eşiğin üstünde olmasını sonraki denetimlerinde devam ediyorsa, koşul çözülene kadar uyarı kuralı yeniden tetiklenmez.

Aşağı "myVM" kullanım geri gelirse bir süre sonra normal, diğer bir deyişle, eşiğin altında gider. Uyarı kuralı koşulu çözümlenen bildirim göndermek için iki birden fazla kez izler. Koşullar kanatların durumunda gürültüsünü azaltmak art arda üç nokta için Uyarı koşulu karşılanmadı zaman uyarı kuralı çözümlendi/devre dışı bir ileti gönderir.

Çözümlenen bildirimi web kancaları ya da e-posta ile gönderilir gibi Azure portalında uyarı örneği (İzleyici durumu olarak adlandırılır) durumunu da çözümlenen ayarlanır.

## <a name="monitoring-at-scale-using-metric-alerts-in-azure-monitor"></a>Uygun ölçekte Azure İzleyicisi'nde ölçüm uyarıları kullanarak izleme

### <a name="using-dimensions"></a>Boyutları kullanma

Azure İzleyici ölçüm uyarıları, ayrıca bir kural ile birden çok boyut değeri bileşimi izlemeyi destekler. Birden çok boyut birleşimleri örneği yardımıyla neden kullanabileceğinize bakalım.

Web siteniz için bir App Service planı olduğunu varsayalım. Web sitesi/uygulama çalıştıran birden fazla CPU kullanımı izlemek istediğiniz. Bir ölçüm uyarısı kuralının gibi kullanarak bunu yapabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Koşul türü: Statik
- Boyutlar
  - Örnek InstanceName1, InstanceName2 =
- Zaman toplama: Ortalama
- Dönem: Son 5 dakika
- Sıklığı: 1 dakika
- İşleç: GreaterThan
- Eşik: 70

Son 5 dakika için ortalama CPU kullanımı % 70'i aşarsa gibi daha önce bu kuralı izler. Ancak aynı kurala iki örnek, Web sitenizi çalıştırmak izleyebilirsiniz. Her örnek tek tek izlenen ve ayrı ayrı bildirimler alırsınız.

Yoğun talep görmesini bir web uygulamasına sahip söyleyin ve daha fazla örnek eklenmesi gerekir. Yukarıdaki kuralı, yalnızca iki örnek yine de izler. Ancak, şu şekilde bir kural oluşturabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Koşul türü: Statik
- Boyutlar
  - Örneği = *
- Zaman toplama: Ortalama
- Dönem: Son 5 dakika
- Sıklığı: 1 dakika
- İşleç: GreaterThan
- Eşik: 70

Bu kural için örnek yani tüm değerleri otomatik olarak izler Ölçüm uyarı kuralınızı yeniden değiştirmek zorunda kalmadan çıktıkça örneklerinizin izleyebilirsiniz.

Birden çok boyutta izlerken, dinamik eşikler uyarı kuralı oluşturabilmeniz ölçüm serisi yüzlerce eşikler aynı anda uyarlanmış. Dinamik eşikler daha az yönetmek için uyarı kuralları ve yönetimi ve Uyarılar kurallar oluşturma hakkında önemli saati sonuçlanıyor.

Birçok örneği ile bir web uygulaması varsa ve en uygun eşiği bilmiyor varsayalım. Yukarıdaki kuralları her zaman eşiğini % 70'i kullanır. Ancak, şu şekilde bir kural oluşturabilirsiniz:

- Hedef kaynak: myAppServicePlan
- Ölçüm: CPU yüzdesi
- Koşul türü: Dinamik
- Boyutlar
  - Örneği = *
- Zaman toplama: Ortalama
- Dönem: Son 5 dakika
- Sıklığı: 1 dakika
- İşleç: GreaterThan
- Duyarlılık: Orta
- Görünüm geri nokta: 1
- İhlal sayısı: 1

Bu kural, son 5 dakika için ortalama CPU kullanımını her örneği için beklenen davranışı aşarsa izler. Aynı kural, ölçüm uyarı kuralı yeniden değiştirmek zorunda kalmadan çıktıkça örneğini izleyebilir. Her olay, ölçüm serisi davranış deseni en uygun bir eşik alın ve sürekli olarak eşiği daha doğru hale getirmek için yeni verilere dayalı değişiklik olur. Gibi daha önce her örneğine ayrı ayrı izlenir ve ayrı ayrı bildirimler alırsınız.

Görünüm sonradan süreleri ve ihlal sayısı artan ayrıca uyarı tanımınızı önemli sapmanın üzerinde yalnızca uyarıları filtrelemeye izin verebilirsiniz. [Dinamik eşikler Gelişmiş seçenekleri hakkında daha fazla bilgi](alerts-dynamic-thresholds.md#what-do-the-advanced-settings-in-dynamic-thresholds-mean).

### <a name="monitoring-multiple-resources-using-metric-alerts"></a>Birden çok kaynak ölçüm uyarıları kullanarak izleme

Önceki bölümde görüldüğü gibi tek tek boyut birleşimi (yani her izleyen tek bir ölçüm uyarısı kuralının olma olasılığı vardır bir ölçüm zaman serisi). Ancak, daha önce bir kerede tek bir kaynak yapmayı hala sınırlıydı. Azure İzleyici ayrıca bir ölçüm uyarısı kuralının ile birden çok kaynak izlenmesini de destekler. Bu özellik şu anda Önizleme ve yalnızca sanal makineler için kullanılabilir. Ayrıca, tek bir ölçüm Uyarısı, bir Azure bölgesindeki kaynaklarını izleyebilir.

Tek bir ölçüm uyarısı üç yoldan biriyle göre izleme kapsamını belirleyebilirsiniz:

- bir aboneliği bir Azure bölgesinde sanal makineler listesi olarak
- bir Abonelikteki bir veya daha fazla kaynak gruplarındaki tüm sanal makineler (bir Azure bölgesinde)
- bir Abonelikteki tüm sanal makineler (bir Azure bölgesinde)

Birden çok kaynak izleme ölçüm uyarı kuralları oluşturma, Azure portalından şu anda desteklenmiyor. Bu kuralları ile oluşturabileceğiniz [Azure Resource Manager şablonları](../../azure-monitor/platform/alerts-metric-create-templates.md#template-for-metric-alert-that-monitors-multiple-resources). Her sanal makine için ayrı bildirim alırsınız.

## <a name="typical-latency"></a>Tipik bir gecikme süresi

Uyarı kuralı sıklığı 1 dakika olarak ayarlarsanız ölçüm uyarıları için genellikle, 5 dakika içinde bildirimi alırsınız. Bildirim sistemleri için ağır yük durumlarda daha uzun bir gecikme süresi görebilirsiniz.

## <a name="supported-resource-types-for-metric-alerts"></a>Desteklenen kaynak türleri için ölçüm uyarıları

Bu konuda desteklenen kaynak türlerini tam listesini bulabilirsiniz [makale](../../azure-monitor/platform/alerts-metric-near-real-time.md#metrics-and-dimensions-supported).

Bugün Klasik ölçüm uyarıları kullanarak ve ölçüm uyarıları tüm kaynak türleri destekleyip desteklemediğini görmek için arıyorsanız kullanmakta olduğunuz, aşağıdaki tabloda kaynak türleri Klasik ölçüm uyarıları tarafından desteklenen ve bunlar tarafından ölçüm uyarıları bugün ya da olmasın desteklenip desteklenmediğini gösterir.

|Klasik ölçüm uyarıları tarafından desteklenen kaynak türü | Ölçüm uyarıları tarafından desteklenir |
|-------------------------------------------------|----------------------------|
| Microsoft.ApiManagement/service | Evet |
| Microsoft.Batch/batchAccounts| Evet|
|Microsoft.Cache/redis| Evet
|Microsoft.ClassicCompute/virtualMachines | Hayır |
|Microsoft.ClassicCompute/domainNames/slots/roles | Hayır|
|Microsoft.CognitiveServices/accounts | Hayır |
|Microsoft.Compute/virtualMachines | Evet|
|Microsoft.Compute/virtualMachineScaleSets| Evet|
|Microsoft.ClassicStorage/storageAccounts| Hayır |
|Microsoft.DataFactory/datafactories | Evet|
|Microsoft.DBforMySQL/servers| Evet|
|Microsoft.DBforPostgreSQL/servers| Evet|
|Microsoft.Devices/ıothubs | Hayır|
|Microsoft.DocumentDB/databaseAccounts| Hayır|
|Microsoft.EventHub/namespaces | Evet|
|Microsoft.Logic/workflows | Evet|
|Microsoft.Network/loadBalancers |Evet|
|Microsoft.Network/publicIPAddresses| Evet|
|Microsoft.Network/applicationGateways| Evet|
|Microsoft.Network/expressRouteCircuits| Evet|
|Microsoft.Network/trafficManagerProfiles | Evet|
|Microsoft.Search/searchServices | Hayır|
|Microsoft.ServiceBus/namespaces| Hayır|
|Microsoft.Storage/storageAccounts | Evet|
|Microsoft.StreamAnalytics/streamingjobs| Evet|
|Microsoft.TimeSeriesInsights/environments | Evet|
|Microsoft. Web/serverfarms | Evet |
|Microsoft. Web/siteleri (işlevler hariç) | Evet|
|Microsoft. HostingEnvironments/Web/multiRolePools | Hayır|
|Microsoft. HostingEnvironments/Web/workerPools| Hayır
|Microsoft.SQL/Servers | Hayır|

## <a name="next-steps"></a>Sonraki adımlar

- [Oluşturun, görüntüleyin ve azure'da ölçüm Uyarıları yönetme hakkında bilgi edinin](alerts-metric.md)
- [Ölçüm uyarıları Azure Resource Manager şablonlarını kullanarak dağıtma hakkında bilgi edinin](../../azure-monitor/platform/alerts-metric-create-templates.md)
- [Eylem grupları hakkında daha fazla bilgi edinin](action-groups.md)
- [Dinamik eşikler koşul türü hakkında daha fazla bilgi edinin](alerts-dynamic-thresholds.md)
