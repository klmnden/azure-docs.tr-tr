---
title: Azure haritalar API kullanımı görüntüleme | Microsoft Docs
description: Portalda, Azure haritalar API çağrıları için ölçümleri görüntüleme öğrenin.
author: walsehgal
ms.author: v-musehg
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 716f9fbc02905a8981771c2fd9dd2be0baeb8196
ms.sourcegitcommit: aa66898338a8f8c2eb7c952a8629e6d5c99d1468
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/28/2019
ms.locfileid: "67460187"
---
# <a name="view-azure-maps-api-usage"></a>Azure haritalar API'si kullanımını görüntüleyin

Bu makalede Azure haritalar hesabınız için API kullanım ölçümleri görüntülemek gösterilmektedir [portalı](https://portal.azure.com). Ölçümler, özelleştirilebilir bir süre boyunca uygun grafik biçiminde gösterilir.

## <a name="view-metric-snapshot"></a>Ölçüm anlık görüntüsünü görüntüle

Bazı ortak ölçümleri gördüğünüz **genel bakış** haritalar hesabınızın sayfası. Şu anda gösterir *toplam istek*, *toplam hata*, ve *kullanılabilirlik* seçilebilir bir süre içinde.

![Azure haritalar ölçümlerine genel bakış](media/how-to-view-api-usage/portal-overview.png)

Belirli çözümleme için bu grafikleri özelleştirmek istiyorsanız sonraki bölüme devam edin.

## <a name="view-detailed-metrics"></a>Ayrıntılı ölçümler görüntüleyin

1. Azure aboneliğinizde oturum açın [portalı](https://portal.azure.com).

2. Tıklayın **tüm kaynakları** menü öğesinin sol tarafında ve gidin, *Azure haritalar hesabı*.

3. Haritalar hesabınız açıldıktan sonra tıklayarak **ölçümleri** sol taraftaki menüden.

4. Üzerinde **ölçümleri** bölmesinde aşağıdakilerden birini seçin:

   1. **Kullanılabilirlik** -hangi gösterir *ortalama* bir süre boyunca API kullanılabilirlik.
   2. **Kullanım** -hangi gösterir nasıl kullanım *sayısı* hesabınız için.

      ![Azure haritalar ölçümleri bölmesi](media/how-to-view-api-usage/portal-metrics.png)

5. Ardından, seçtiğiniz *zaman aralığı* tıklayarak **son 24 saat (otomatik)** . Varsayılan olarak, zaman aralığı 24 saate ayarlanır. Tıklandıktan sonra tüm seçilebilir zaman aralıkları görürsünüz. Seçebileceğiniz *zaman ayrıntı düzeyi* ve zaman olarak görüntülemeyi *yerel* veya *GMT* aynı açılan. **Uygula**'ya tıklayın.

    ![Azure haritalar ölçümleri zaman aralığı](media/how-to-view-api-usage/time-range.png)

6. Ölçümünüzün ekledikten sonra böylece **Filtre Ekle** devralmak için ilgili özellikleri kabuş edin ve ardından istediğiniz özelliğinin değeri için grafiği görmek.

    ![Azure haritalar ölçümleri filtresi](media/how-to-view-api-usage/filter.png)

7. Ayrıca **uygulamak bölme** seçilen ölçüm özelliğini temel alarak, ölçüm için. Bu, her biri bu özelliğin her değeri için birden çok grafik bölünmesi için grafiği sağlar. Aşağıdaki resimde, grafiğin alt kısmında gösterilen özellik değerinin her grafik rengini karşılık gelir.

    ![Azure haritalar bölme ölçümleri](media/how-to-view-api-usage/splitting.png)

8. Birden çok ölçümleri aynı grafikte üzerinde yalnızca tıklayarak mümkün da **ölçüm Ekle** üstteki düğmesine.

## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar kullanım için izlemek istediğiniz API'ları hakkında daha fazla bilgi edinin:
> [!div class="nextstepaction"] 
> [Azure haritalar Web SDK'nasıl yapılır konuları](how-to-use-map-control.md)

> [!div class="nextstepaction"] 
> [Azure haritalar Android SDK'sını nasıl yapılır konuları](how-to-use-android-map-control-library.md)

> [!div class="nextstepaction"]
> [Azure haritalar REST API'si belgeleri](https://docs.microsoft.com/rest/api/maps)
