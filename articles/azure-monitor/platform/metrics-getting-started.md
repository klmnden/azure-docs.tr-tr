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
ms.openlocfilehash: 9d23d4b30ca4d394fb4afd0bb6620be6df179600
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57444948"
---
# <a name="getting-started-with-azure-metrics-explorer"></a>Azure ölçüm Gezgini ile çalışmaya başlama

## <a name="where-do-i-start"></a>Nereden başlamalıyım

> [!NOTE]
> Bu makalede, Azure İzleyici ölçüm Gezgini ile çalışmaya başlama yeni kullanıcılara yardımcı olmak için temel kavramları kapsar. Daha ayrıntılı belgelere ve Gelişmiş grafik ayarlarında ve ölçümlerinde hakkında bilgi için bkz. [Gelişmiş Özellikler, Azure İzleyici ölçüm Gezgini'ni](https://docs.microsoft.com/azure/azure-monitor/platform/metrics-charts).

Sistem durumu ve kaynaklarınızın kullanımını araştırmak için ölçüm Gezgini'ni kullanın. Aşağıdaki sırayla başlatın:

1. Başlayın [kaynak ve bir ölçüm çekme](#creating-your-first-metric-chart) ve temel bir grafik görürsünüz. Ardından [bir zaman aralığı seçin](#picking-time-range) araştırmanızı için ilgili olmasıdır.

1. Deneyin [boyut filtreleri uygulayarak ve bölme](#applying-dimension-filters-and-splitting). Filtreler ve hangi segmentlerin ölçümün genel ölçüm değerine katkıda analiz etmenize izin vermek ve olası aykırı değerleri tanımlamayı bölme.

1. Kullanım [Gelişmiş ayarlar](#advanced-chart-settings-and-next-steps) panolara sabitlemeden önce grafiği özelleştirme. [Uyarıları Yapılandır](alerts-metric-overview.md) ölçüm değeri aşarsa veya bir eşiğin altına düşünceye bildirimleri almak için.

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

## <a name="pick-a-time-range"></a>Bir zaman aralığı seçin

Varsayılan olarak, en son 24 saat ölçümlerini veri grafik gösterir. Kullanım **Saat Seçici** zaman aralığını değiştirmek, yakınlaştırmak veya grafiğinizde uzaklaştırmak için paneli. 

![Değişiklik zaman aralığı paneli](./media/metrics-getting-started/time-picker.png)

## <a name="apply-dimension-filters-and-splitting"></a>Boyut filtreleri ve bölme uygulayın

[Filtreleme](metrics-charts.md#apply-filters-to-charts) ve [bölme](metrics-charts.md#apply-splitting-to-a-chart) boyutlara sahip ölçümleri için güçlü tanılama araçları. Bu özellik, çeşitli ölçüm parçaları ("boyut değerleri") ölçüm öğenin toplam değeri etkiler ve olası aykırı değerleri tanımlamayı sağlar gösterir.

- **Filtreleme** seçtiğiniz boyut değerlerini grafikte yer sağlar. Örneğin, grafik, başarılı istekler göstermek isteyebilirsiniz *sunucu yanıt süresi* ölçümü. Filtre uygulamak gerekir *istek başarısı* boyut. 

- **Bölme** grafik ayrı görüntüler satırları bir boyut için her bir değer veya tek bir satıra değerleri toplar denetim. Örneğin, bir satır için bir ortalama yanıt süresi, tüm sunucu örneklerinde bakın veya her sunucu için ayrı satırlara bakın. Temel bölme uygulanacak gerekir *sunucuyu* ayrı satırlara görmek için boyut.

Bkz: [grafikleri örnekleri](metric-chart-samples.md) filtreleme ve bölme uyguladınız. Makalede grafikleri yapılandırmak için kullanılan adımları gösterilmektedir.

## <a name="advanced-chart-settings-and-next-steps"></a>Gelişmiş grafik ayarları ve sonraki adımlar

Grafik stili, başlık, özelleştirebilir ve Gelişmiş grafik ayarlarını değiştirin. Özelleştirme ile işiniz bittiğinde, çalışmanızı kaydetmek için bir panoya sabitleyebilirsiniz. Ölçüm uyarıları da yapılandırabilirsiniz. İzleyin [ürün belgelerine](metrics-charts.md) bu ve diğer hakkında bilgi edinmek için Azure İzleyici ölçüm Gezgini'ni özelliklerinin Gelişmiş.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Hizmetleri için kullanılabilir ölçümleri bakın](metrics-supported.md)
* [Ölçüm Gezgini'nde gelişmiş özellikler hakkında bilgi edinin](metrics-charts.md)
* [Yapılandırılmış grafiklerin örneklerine bakın](metric-chart-samples.md)
