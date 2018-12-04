---
title: Tanılama ve Azure Time Series Insights (Önizleme) ilgili sorunları giderme | Microsoft Docs
description: Tanılama ve Azure Time Series Insights (Önizleme) ile ilgili sorunları giderme anlama
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 11/27/2018
ms.openlocfilehash: d13d373169287a0ec5931d5437b0a3bc70ecd79a
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856852"
---
# <a name="how-to-diagnose-and-troubleshoot"></a>Tanılama ve sorun giderme

Bu makalede, Azure zaman serisi öngörüleri (TSI) ortamınızla çalışma karşılaşabileceğiniz bazı yaygın sorunlar özetlenmektedir. Makalede ayrıca olası nedenler ve çözümler için her açıklanır.

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Sorun: Veri Time Series Insights Gezgini (Önizleme) görülür.

Neden Azure TSI Gezgini verilerinizdeki göremeyebilirsiniz birkaç genel nedeni vardır:

1. Olay kaynağınızı veri alma değil.

    Olay kaynağınızı (olay hub'ı veya IOT hub'ı), etiketler veri aldığını doğrulayın / örnekler. Azure portalında kaynağınıza Genel Bakış sayfasına giderek bunu yapabilirsiniz.

    ![ınsights Panosu][1]

1. Olay kaynağı verilerinizi JSON biçiminde değil.

    Azure TSI yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

1. Olay kaynağı anahtarınızı gerekli izni yok

    ![yapılandırma][2]

    * IOT Hub hizmetine bağlanma iznine sahip olan anahtar belirtmeniz gerekir.
    * Yukarıdaki görüntüde, ilkelerin ya da gösterildiği gibi *iothubowner* ve çalışma hizmeti, çünkü her ikisi de hizmet connect izni yok.
    * Bir event hub için dinleme izni olan bir anahtar belirtmeniz gerekir.
    * Önceki görüntüde gösterildiği gibi ilkelerin ya da okuma ve yönetme çalışır, çünkü her ikisi de dinleme izne sahip.

    ![izinler][3]

1. Sağlanan tüketici grubunuz için TSI özel değildir.

    IOT hub'ı kaydı veya bir olay hub'ı sırasında verileri okumak için kullanılması gereken tüketici grubu belirtin. Bu tüketici grubunun Paylaşılmaması gerekir. Tüketici grubu paylaşılıyorsa, temel alınan olay hub'ı otomatik olarak bir okuyucu rastgele bağlantısını keser. Okunacak TSI için bir benzersiz bir tüketici grubu sağlayın.

## <a name="problem-some-data-is-shown-but-some-is-missing"></a>Sorun: Bazı veriler gösterilir, ancak bazı eksik

Ortamınızı kısıtladığı için bu durum oluşabilir.

> [!NOTE]
> Şu anda Azure TSI 6 MB/sn en yüksek alma oranını destekler.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Sorun: Benim olay kaynağının zaman damgası özellik adı ayarı çalışmaz.

Ad ve değer şu kurallara uyar emin olun:

* **Zaman damgası** özellik adı büyük/küçük harfe duyarlıdır.
* **Zaman damgası** , olay kaynağınızdaki bir JSON dizesi olarak gelen bir özellik değeri biçiminde olmalıdır `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Bu tür bir dize örneğidir `“2008-04-12T12:53Z”`.

Zaman damgası özellik adı, yakalanan ve TSI Gezgini kullanmak için düzgün bir şekilde çalışıyor olduğundan emin olmak için en kolay yolu. TSI Gezgini içinde bir süre sonra size sağlanan zaman damgası özellik adı seçmek için grafik kullanın. Seçime sağ tıklayın ve seçin **olayları keşfet** seçeneği. İlk sütun üst bilgisi olmalıdır, **zaman damgası** özellik adı ve olmalıdır bir `($ts)` sözcüğünün yanındaki `Timestamp`, yerine:

* `(abc)`, veri değerleri dize olarak okuma, TSI gösterir
* TSI, veri değeri datetime olarak okuyor gösterir Takvim simgesine
* `#`, TSI belirtmek veri değerlerini bir tamsayı olarak okuyor

## <a name="problem-my-time-series-id-property-is-incorrect-missing-or-null"></a>Sorun: My zaman serisi ID özelliği, yanlış, eksik veya null.

Bu meydana gelmiş olabilir **zaman serisi kimliği** özelliği hatalı ortamı sağlama sırasında yapılandırılır. Bkz: [bir zaman serisi kimliği seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md) makale seçerken bir **zaman serisi kimliği**. Şu anda farklı bir kullanmak için mevcut TSI (Önizleme) ortamında güncelleştirilemiyor **zaman serisi kimliği**.

## <a name="problem-all-my-instances-in-time-series-insights-preview-explorer-dont-have-a-parent"></a>Sorun: Tüm Örneklerim Time Series Insights Gezgini (Önizleme) içinde bir üst öğe yok

Ortamınızı sahip değilse bu durum ortaya çıkabilir bir **zaman serisi modeli** tanımlanan hiyerarşisi. Daha fazla bilgi için bu makaleye bakın [zaman serisi modelleri ile çalışma konusunda](./time-series-insights-update-how-to-tsm.md).

## <a name="next-steps"></a>Sonraki adımlar

* Okuma [zaman serisi modelleri ile çalışma konusunda](./time-series-insights-update-how-to-tsm.md).

* Okuma [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[2]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[3]: media/v2-update-diagnose-and-troubleshoot/permissions.png