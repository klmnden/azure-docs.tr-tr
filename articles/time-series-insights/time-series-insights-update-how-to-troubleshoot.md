---
title: Tanılama ve sorun giderme Azure zaman serisi öngörüleri Önizleme | Microsoft Docs
description: Azure zaman serisi öngörüleri önizlemesi ile ilgili sorunları giderme ve Tanılama hakkında bilgi edinin.
author: ashannon7
ms.author: anshan
ms.workload: big-data
manager: cshankar
ms.service: time-series-insights
services: time-series-insights
ms.topic: conceptual
ms.date: 12/06/2018
ms.custom: seodec18
ms.openlocfilehash: 3ab3c680f7279ff78e0319f28f67c1cc8c203b47
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64708025"
---
# <a name="diagnose-and-troubleshoot"></a>Tanılama ve sorun giderme

Bu makalede, Azure zaman serisi öngörüleri önizlemesi ortamınız ile çalışırken karşılaşabileceğiniz bazı yaygın sorunlar özetlenmektedir. Makalede ayrıca olası nedenler ve çözümler her sorun için açıklanır.

## <a name="problem-i-cant-find-my-environment-in-the-time-series-insights-preview-explorer"></a>Sorun: Zaman serisi öngörüleri Önizleme Gezgini'nde ortamımın bulunamıyor

Zaman serisi görüşleri ortamına erişim izni yoksa, bu sorun oluşabilir. Kullanıcıların kendi Time Series Insights ortamı görüntülemek için bir okuyucu düzeyinde erişim rolünü gerekir. Geçerli erişim düzeyleri doğrulamak ve ek erişim vermek için zaman serisi görüşleri kaynak veri erişimi ilkeleri bölümü ziyaret [Azure portalında](https://portal.azure.com/).

  ![Ortam][1]

## <a name="problem-no-data-is-seen-in-the-time-series-insights-preview-explorer"></a>Sorun: Veri yok zaman serisi öngörüleri Önizleme Gezgini'nde görülür.

Neden olmayan görebileceğiniz verilerinizi birkaç genel nedeni vardır [Azure zaman serisi öngörüleri önizlemesi Gezgini](https://insights.timeseries.azure.com/preview).

- Olay kaynağınızı veri alma değil.

    Bir olay hub'ı veya IOT hub, olay kaynağı veri etiketleri ya da örnek aldığını doğrulayın. Doğrulamak için Azure portalında kaynağınıza Genel Bakış sayfasına gidin.

    ![ınsights Panosu][2]

- Olay kaynağı verileriniz, JSON biçiminde değil.

    Time Series Insights, yalnızca JSON verilerini destekler. JSON örnekleri için bkz [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

- Olay kaynağı anahtarınızı gerekli izni yok.

  * Bir IOT hub için anahtar sağlamanız gereken **hizmetini bağlama** izni.

    ![Yapılandırma][3]

  * Yukarıdaki görüntüde, iki ilke gösterildiği **iothubowner** ve **hizmet** sahip oldukları çalışma **hizmetine bağlanın** izni.
  * Bir event hub için anahtar sağlamanız gereken **dinleme** izni.
  
    ![İzinler][4]

  * Yukarıdaki görüntüde, her ikisi de gösterildiği gibi **okuma** ve **yönetme** ilkeleri, sahip oldukları çalışma **dinleme** izni.

- Sağlanan tüketici grubunuz için Time Series Insights özel değildir.

    Bir IOT hub'ı veya olay hub'ı, kayıt sırasında verileri okumak için kullanılan bir tüketici grubu belirtin. Bu tüketici grubu paylaşmayın. Tüketici grubu paylaşılıyorsa, temel alınan olay hub'ı otomatik olarak bir okuyucu rastgele bağlantısını keser. Time Series Insights'ın okumak için bir benzersiz bir tüketici grubu sağlayın.

- Zaman serisi kimliği özelliğinizi sağlama sırasında belirtilen yanlış, eksik veya null.

    Zaman serisi ID özelliği ortamı sağlama sırasında yanlış yapılandırılırsa, bu sorun ortaya çıkabilir. Daha fazla bilgi için [bir zaman serisi kimliği seçmek için en iyi yöntemler](./time-series-insights-update-how-to-id.md). Şu anda farklı bir zaman serisi kimliği kullanmak için bir zaman serisi görüşleri ortamı güncelleştirilemiyor.

## <a name="problem-some-data-shows-but-some-is-missing"></a>Sorun: Bazı verileri gösterir, ancak bazı eksik

Zaman serisi kimliği olmadan veri gönderiyor olabilir

- Olayları zaman serisi Kimliği alanı yükü olmadan gönderdiğinizde bu sorun oluşabilir. Daha fazla bilgi için [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

- Ortamınızı kısıtladığı için bu sorun oluşabilir.

    > [!NOTE]
    > Şu anda 6 MB/sn en yüksek alma oranını Time Series Insights'ı destekler.

## <a name="problem-my-event-sources-timestamp-property-name-setting-doesnt-work"></a>Sorun: My olay kaynağının zaman damgası özellik adı ayarı çalışmaz

Ad ve değer şu kurallara uyar emin olun:

* Zaman damgası özellik adı büyük/küçük harfe duyarlıdır.
* Bir JSON dizesi olarak olay kaynağınızı geldiği zaman damgası özellik değeri şu biçimdedir `yyyy-MM-ddTHH:mm:ss.FFFFFFFK`. Bu tür bir dize örneğidir `“2008-04-12T12:53Z”`.

Zaman damgası özellik adı, yakalanan ve zaman serisi öngörüleri Önizleme Gezgini'ni kullanma düzgün bir şekilde çalışıyor olduğundan emin olmak için en kolay yolu. Zaman serisi öngörüleri Önizleme Gezgini içinde bir süre sonra size sağlanan zaman damgası özellik adı seçmek için grafik kullanın. Seçime sağ tıklayın ve seçin **olayları keşfet** seçeneği. İlk sütun başlığına, zaman damgası özellik adı ' dir. Olması gereken `($ts)` sözcüğünün yanındaki `Timestamp`, yerine:

* `(abc)`, gösteren Time Series Insights veri değerleri dize olarak okur.
* Takvim simgesini gösteren Time Series Insights veri değerini datetime olarak okur.
* `#`, belirten bir tamsayı olarak Time Series Insights veri değerlerini okur.

Zaman damgası özelliği açıkça belirtilmezse, bir olayın IOT hub'ı veya olay hub'ı sıraya süresi varsayılan zaman damgası kullanılır.

## <a name="problem-i-cant-edit-or-view-my-time-series-model"></a>Sorun: Düzenleyemiyor veya my zaman serisi modeli görüntüleyin

- Zaman serisi öngörüleri S1 veya S2 ortam erişiyor.

   Zaman serisi modelleri yalnızca PAYG ortamlarında desteklenir. Zaman serisi öngörüleri Önizleme Gezgini'nden S1/S2 ortamınızı erişim hakkında daha fazla bilgi için bkz. [gezginde verileri görselleştirme](./time-series-insights-update-explorer.md).

   ![Access][5]

- Görüntüleme ve düzenleme model izni olmayabilir.

   Kullanıcıların, düzenlemek ve görüntülemek, zaman serisi modeli için katkıda bulunan düzeyinde erişim gerekir. Geçerli erişim düzeyleri doğrulamak ve ek erişim vermek için Azure portalında Time Series Insights kaynağınızda veri erişimi ilkeleri bölümünü ziyaret edin.

## <a name="problem-all-my-instances-in-the-time-series-insights-preview-explorer-dont-have-a-parent"></a>Sorun: Zaman serisi öngörüleri Önizleme Gezgini'nde tüm Örneklerim üst öğesi yok

Ortamınızı tanımlı bir zaman serisi modeli hiyerarşi yok, bu sorun ortaya çıkabilir. Daha fazla bilgi için [çalışma ile zaman serisi modelleri](./time-series-insights-update-how-to-tsm.md).

  ![Zaman serisi modelleri][6]

## <a name="next-steps"></a>Sonraki adımlar

- Okuma [çalışma ile zaman serisi modelleri](./time-series-insights-update-how-to-tsm.md).

- Hakkında bilgi edinin [desteklenen JSON şekilleri](./how-to-shape-query-json.md).

<!-- Images -->
[1]: media/v2-update-diagnose-and-troubleshoot/environment.png
[2]: media/v2-update-diagnose-and-troubleshoot/dashboard-insights.png
[3]: media/v2-update-diagnose-and-troubleshoot/configuration.png
[4]: media/v2-update-diagnose-and-troubleshoot/permissions.png
[5]: media/v2-update-diagnose-and-troubleshoot/access.png
[6]: media/v2-update-diagnose-and-troubleshoot/tsm.png