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
ms.date: 12/06/2018
ms.openlocfilehash: 4fdb7cf0007af5f92794ebe5f616c1c8a28af0e4
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/08/2018
ms.locfileid: "53099674"
---
# <a name="how-to-diagnose-and-troubleshoot"></a>Tanılama ve sorun giderme

Bu makalede, Azure zaman serisi öngörüleri (TSI) ortamınızla çalışma karşılaşabileceğiniz bazı yaygın sorunlar özetlenmektedir. Makalede ayrıca olası nedenler ve çözümler için her açıklanır.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Sorun: ortamımın Time Series Insights (Önizleme) Gezgini'nde bulamıyorum

TSI ortam erişim izni yoksa, bu durum oluşabilir. Kullanıcıların kendi TSI ortamı görüntülemek için "okuyucu" düzeyinde erişim rolünü gerekir. Geçerli erişim düzeyleri doğrulayın ve TSI kaynak üzerinde veri erişim ilkeler bölümünü ziyaret ederek ek erişim [Azure portalı](https://portal.azure.com/).

  ![environment][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Sorun: Veri yok Time Series Insights (Önizleme) Gezgini'nde görülür.

Neden olmayan görebileceğiniz verilerinizi birkaç genel nedeni vardır [(Önizleme) Azure TSI Gezgini](https://insights.timeseries.azure.com/preview):

1. Olay kaynağınızı veri alma değil.

    Olay kaynağınızı (olay hub'ı veya IOT hub'ı), etiketler veri aldığını doğrulayın / örnekler. Azure portalında kaynağınıza Genel Bakış sayfasına giderek bunu yapabilirsiniz.

    ![ınsights Panosu][2]

1. Olay kaynağı verilerinizi JSON biçiminde değil.

    Azure TSI yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

1. Olay kaynağı anahtarınızı gerekli izni yok

    * IOT Hub hizmetine bağlanma iznine sahip olan anahtar belirtmeniz gerekir.

    ![yapılandırma][3]

    * Yukarıdaki görüntüde, ilkelerin ya da gösterildiği gibi *iothubowner* ve çalışma hizmeti, çünkü her ikisi de hizmet connect izni yok.
    * Bir event hub için dinleme izni olan bir anahtar belirtmeniz gerekir.
  
    ![izinler][4]

    * Yukarıdaki görüntüde, ilkelerin ya da gösterildiği gibi **okuma** ve **yönetme** çalışır, çünkü her ikisi de **dinleme** izni.

1. Sağlanan tüketici grubunuz için TSI özel değildir.

    Bir IOT hub'ı veya olay hub'ı, kayıt sırasında veri okumak için kullanılması gereken tüketici grubu belirtin. Bu tüketici grubu Paylaşılmaması gerekir. Tüketici grubu paylaşılıyorsa, temel alınan olay hub'ı otomatik olarak bir okuyucu rastgele bağlantısını keser. Okunacak TSI için bir benzersiz bir tüketici grubu sağlayın.

1. Zaman serisi kimliği özelliğinizi sağlama sırasında belirtilen yanlış, eksik veya null

    Bu meydana gelmiş olabilir **zaman serisi kimliği** özelliği hatalı ortamı sağlama sırasında yapılandırılır. Lütfen [bir zaman serisi kimliği seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md). Şu anda farklı bir kullanmak için bir zaman serisi görüşleri güncelleştirme ortamı güncelleştirilemiyor **zaman serisi kimliği**.

## <a name="problem-some-data-is-shown-but-some-is-missing"></a>Sorun: Bazı veriler gösterilir, ancak bazı eksik

1. Zaman serisi kimliği olmadan veri gönderme

    Zaman serisi Kimliği alanı yükü olmadan olayları gönderiyorsunuz bu hata oluşabilir. Bkz: [desteklenen JSON şekilleri](./how-to-shape-query-json.md) daha fazla bilgi için.

1. Ortamınızı kısıtladığı için bu durum oluşabilir.

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

Varsa **zaman damgası** özelliği açıkça belirtilmediyse, bir olayın IOT hub'ı veya olay hub'ı yararlanılacaktır **sıraya zaman** varsayılan zaman damgası olarak.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Sorun: düzenleyemiyor veya my zaman serisi modeli görüntüleyin

1. Bir zaman serisi öngörüleri S1 veya S2 ortam erişiyor olabilir

   Zaman serisi modelleri yalnızca desteklenen **PAYG** ortamları. Zaman serisi öngörüleri Önizleme Gezgini'nden S1/S2 ortamınızı erişim hakkında daha fazla bilgi için bkz. [gezginde verileri görselleştirme](./time-series-insights-update-explorer.md).

   ![erişim][5]

1. Görüntüleme ve modelin düzenleme iznine sahip değil

   Kullanıcıların, düzenlemek ve kendi zaman serisi modeli görüntülemek için "katılımcı" düzeyinde erişimi gerekir. Geçerli erişim düzeyleri doğrulayın ve Azure Portalı'nda Time Series Insights kaynağınızda veri erişim ilkeler bölümünü ziyaret ederek ek erişim.

## <a name="problem-all-my-instances-in-time-series-insights-preview-explorer-dont-have-a-parent"></a>Sorun: Tüm Örneklerim Time Series Insights (Önizleme) Gezgini'nde bir üst öğe yok

Ortamınızı sahip değilse bu durum ortaya çıkabilir bir **zaman serisi modeli** tanımlanan hiyerarşisi. Daha fazla bilgi için bu makaleye bakın [zaman serisi modelleri ile çalışma konusunda](./time-series-insights-update-how-to-tsm.md).

  ![TSM][6]

## <a name="next-steps"></a>Sonraki adımlar

Okuma [zaman serisi modelleri ile çalışma konusunda](./time-series-insights-update-how-to-tsm.md).

Okuma [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png