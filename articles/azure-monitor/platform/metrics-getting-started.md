---
title: Azure ölçüm Gezgini ile çalışmaya başlama
description: Azure ölçüm Gezgini ile ilk ölçüm grafiğini oluşturmayı öğrenin.
author: vgorbenko
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 02/25/2019
ms.author: vitalyg
ms.subservice: metrics
ms.openlocfilehash: 3306e888970d99132d17d4ccf967f074302412ca
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/13/2019
ms.locfileid: "65595444"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Azure ölçüm Gezgini ile çalışmaya başlama

## <a name="where-do-i-start"></a>Nereden başlamalıyım
Azure İzleyici ölçüm Gezgini'ni çizim grafikleri, görsel olarak eğilimleri ilişkilendirme ve ani araştırma sağlar ve düşüşler ölçümleri değerleri Microsoft Azure portalının bir bileşenidir. Sistem durumu ve kaynaklarınızın kullanımını araştırmak için ölçüm Gezgini'ni kullanın. Aşağıdaki sırayla başlatın:

1. [Kaynak ve bir ölçüm seçin](#create-your-first-metric-chart) ve temel bir grafik görürsünüz. Ardından [bir zaman aralığı seçin](#select-a-time-range) araştırmanızı için ilgili olmasıdır.

1. Deneyin [boyut filtreleri uygulayarak ve bölme](#apply-dimension-filters-and-splitting). Filtreler ve hangi segmentlerin ölçümün genel ölçüm değerine katkıda analiz etmenize izin vermek ve olası aykırı değerleri tanımlamayı bölme.

1. Kullanım [Gelişmiş ayarlar](#advanced-chart-settings) panolara sabitlemeden önce grafiği özelleştirme. [Uyarıları Yapılandır](alerts-metric-overview.md) ölçüm değeri aşarsa veya bir eşiğin altına düşünceye bildirimleri almak için.

## <a name="create-your-first-metric-chart"></a>Ölçüm ilk grafiğinizi oluşturun

Bir ölçüm grafiği, kaynak, kaynak grubu, abonelik veya Azure İzleyici görünümü oluşturmak için açık **ölçümleri** sekme ve aşağıdaki adımları izleyin:

1. Kaynak seçiciyi kullanarak, görmek istediğiniz kaynağı seçin. (Kaynak açtıysanız önceden seçilmiş **ölçümleri** belirli bir kaynağa bağlamında).

    > ![Bir kaynak seçin](./media/metrics-getting-started/resource-picker.png)

2. Bazı kaynaklar için bir ad alanı seçmeniz gerekir. Ad alanı ölçümleri kolayca bulabilmesi için düzenlemek için yalnızca bir yoludur. Örneğin, depolama hesapları, dosyaları, tablolar, BLOB'lar ve Kuyruklar ölçümleri depolamak için ayrı ad alanları sahip. Birçok kaynak türleri yalnızca bir ad alanı var.

3. Bir ölçüm, kullanılabilir ölçümler listesinden seçin.

    > ![Bir ölçüm seçin](./media/metrics-getting-started/metric-picker.png)

4. İsteğe bağlı olarak, ölçüm toplama değiştirebilirsiniz. Örneğin, ölçüm en düşük, en yüksek ve ortalama değerleri göstermek için grafik isteyebilirsiniz.

> [!NOTE]
> Kullanım **ölçüm Ekle** düğmesine tıklayın ve birden çok ölçümleri aynı grafikte çizilen görmek istiyorsanız, bu adımları yineleyin. Tek bir görünümde birden çok grafiklerde seçin **Ekle grafik** üstteki düğmesine.

## <a name="select-a-time-range"></a>Bir zaman aralığı seçin

Varsayılan olarak, en son 24 saat ölçümlerini veri grafik gösterir. Kullanım **Saat Seçici** zaman aralığını değiştirmek, yakınlaştırmak veya grafiğinizde uzaklaştırmak için paneli. 

![Değişiklik zaman aralığı paneli](./media/metrics-getting-started/time-picker.png)

## <a name="apply-dimension-filters-and-splitting"></a>Boyut filtreleri ve bölme uygulayın

[Filtreleme](metrics-charts.md#apply-filters-to-charts) ve [bölme](metrics-charts.md#apply-splitting-to-a-chart) boyutlara sahip ölçümleri için güçlü tanılama araçları. Bu özellikler, ölçüm öğenin toplam değeri çeşitli ölçüm parçaları ("boyut değerleri") etkisini göstermek ve olası aykırı değerleri tanımlamayı sağlar.

- **Filtreleme** seçtiğiniz boyut değerlerini grafikte yer sağlar. Örneğin, grafik, başarılı istekler göstermek isteyebilirsiniz *sunucu yanıt süresi* ölçümü. Filtre uygulamak gerekir *istek başarısı* boyut. 

- **Bölme** grafik ayrı görüntüler satırları bir boyut için her bir değer veya tek bir satıra değerleri toplar denetim. Örneğin, bir satır için bir ortalama yanıt süresi, tüm sunucu örneklerinde bakın veya her sunucu için ayrı satırlara bakın. Temel bölme uygulanacak gerekir *sunucuyu* ayrı satırlara görmek için boyut.

Bkz: [grafikleri örnekleri](metric-chart-samples.md) filtreleme ve bölme uyguladınız. Makalede grafikleri yapılandırmak için kullanılan adımları gösterilmektedir.

## <a name="advanced-chart-settings"></a>Gelişmiş grafik ayarları

Grafik stili, başlık, özelleştirebilir ve Gelişmiş grafik ayarlarını değiştirin. Özelleştirme ile işiniz bittiğinde, çalışmanızı kaydetmek için bir panoya sabitleyebilirsiniz. Ölçüm uyarıları da yapılandırabilirsiniz. İzleyin [ürün belgelerine](metrics-charts.md) bu ve diğer hakkında bilgi edinmek için Azure İzleyici ölçüm Gezgini'ni özelliklerinin Gelişmiş.

## <a name="next-steps"></a>Sonraki adımlar

* [Ölçüm Gezgini gelişmiş özellikler hakkında bilgi edinin](metrics-charts.md)
* [Ölçüm Gezgini sorunlarını giderme](metrics-troubleshoot.md)
* [Azure Hizmetleri için kullanılabilir ölçümleri bakın](metrics-supported.md)
* [Yapılandırılmış grafiklerin örneklerine bakın](metric-chart-samples.md)
