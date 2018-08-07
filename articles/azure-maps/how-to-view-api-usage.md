---
title: Azure haritalar API kullanımı görüntüleme | Microsoft Docs
description: Portalda, Azure haritalar API çağrıları için ölçümleri görüntüleme öğrenin.
author: dsk-2015
ms.author: dkshir
ms.date: 08/06/2018
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: timlt
ms.openlocfilehash: 9eb58f41ec89d084cc388eeadb335046fb587bc3
ms.sourcegitcommit: 615403e8c5045ff6629c0433ef19e8e127fe58ac
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2018
ms.locfileid: "39581896"
---
# <a name="how-to-view-the-azure-maps-api-usage"></a>Azure haritalar API kullanımı görüntüleme
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

    1. **Kullanılabilirlik**, hangi gösterir *ortalama* API kullanılabilirlik bir süre boyunca, veya 
    2. **Kullanım**, hangi gösterir nasıl kullanım *sayısı* hesabınız için. 

    ![Azure haritalar ölçümleri bölmesi](media/how-to-view-api-usage/portal-metrics.png)

5. Ölçümler seçtikten sonra seçtiğiniz *zaman aralığı* seçerek **son 12 saat (otomatik)** varsayılan değer olan. Ayrıca seçebilirsiniz *zaman ayrıntı düzeyi*, yanı sıra saati olarak görüntülemeyi *yerel* veya *GMT* aynı açılan. **Uygula**'ya tıklayın.

    ![Azure haritalar ölçümleri zaman aralığı](media/how-to-view-api-usage/time-range.png)
 
6. Ölçümünüzün ekledikten sonra böylece **Filtre Ekle** devralmak için ilgili özellikleri kabuş edin ve ardından istediğiniz özelliğinin değeri için grafiği görmek. 

    ![Azure haritalar ölçümleri filtresi](media/how-to-view-api-usage/filter.png)

7. Ayrıca **uygulamak bölme** seçilen ölçüm özelliğini temel alarak, ölçüm için. Bu, her biri bu özelliğin her değeri için birden çok grafik bölünmesi için grafiği sağlar. Örneğin, aşağıdaki resimde, her grafik rengini grafiğin alt kısmında gösterilen özellik değerine karşılık gelir.

    ![Azure haritalar bölme ölçümleri](media/how-to-view-api-usage/splitting.png)
 
8. Birden çok ölçümleri aynı grafikte üzerinde yalnızca tıklayarak mümkün da **ölçüm Ekle** üstteki düğmesine.


## <a name="next-steps"></a>Sonraki adımlar

Azure haritalar kullanımınızı izleyin gerçekleştirmeyi öğrendiniz, daha gelişmiş özelliklerinden gibi öğrenmek için ilerleyebilirsiniz:

* [Araç tüketim](consumption-model.md), veya
* [GeoJSON genişletme](extend-geojson.md)

Son olarak, daha fazla bilgi ile kullandığınız API'leri hakkında:
* [Azure haritalar REST API'si belgeleri](https://docs.microsoft.com/rest/api/maps)
