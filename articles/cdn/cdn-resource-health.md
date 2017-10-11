---
title: "Azure CDN kaynakları sağlığını izlemek | Microsoft Docs"
description: "Azure kaynak durumu kullanarak Azure CDN kaynaklarınızı sağlığını izlemek öğrenin."
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: 
ms.assetid: bf23bd89-35b2-4aca-ac7f-68ee02953f31
ms.service: cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 37fe208f5087f318e665e76825127854b4a11c98
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2017
---
# <a name="monitor-the-health-of-azure-cdn-resources"></a>Azure CDN kaynakların durumunu izleme
  
Azure CDN kaynak durumu olan bir alt kümesini [Azure kaynak durumu](../resource-health/resource-health-overview.md).  Azure kaynak durumu CDN kaynakları sağlığını izlemek ve sorunlarını gidermek için işlem yapılabilir yönergeleri almak için kullanabilirsiniz.

>[!IMPORTANT] 
>Azure CDN kaynak durumu şu anda yalnızca sistem durumunu genel CDN teslimi ve API özellikleri için hesapları.  Azure CDN kaynak durumu tek tek CDN uç noktası doğrulamaz.
>
>Azure CDN kaynak durumu akışı sinyalleri Gecikmeli en fazla 15 dakika olabilir.

## <a name="how-to-find-azure-cdn-resource-health"></a>Azure CDN kaynak sistem durumu bulma

1. İçinde [Azure portal](https://portal.azure.com), CDN profilinize gidin.

2. Tıklatın **ayarları** düğmesi.

    ![Ayarlar düğmesi](./media/cdn-resource-health/cdn-profile-settings.png)

3. Altında *destek + sorun giderme*, tıklatın **kaynak durumu**.

    ![CDN kaynak durumu](./media/cdn-resource-health/cdn-resource-health3.png)

>[!TIP] 
>Listelenen CDN kaynakları bulabileceğiniz *kaynak durumu* parçasında *Yardım + Destek* dikey.  Hızlı bir şekilde elde edebilirsiniz *Yardım + Destek* daire içinde tıklayarak **?** Portalın sağ üst köşesinde.
>
> ![Yardım + destek](./media/cdn-resource-health/cdn-help-support.png)

## <a name="azure-cdn-specific-messages"></a>Azure CDN özgü iletileri

Azure CDN kaynak sağlığı ile ilgili durumlar altında bulunabilir.

|İleti | Önerilen eylem |
|---|---|
|Durdurulmuş, kaldırıldı veya bir veya daha fazla CDN uç noktalarınızı yanlış | Durduruldu, kaldırıldı veya bir veya daha fazla CDN uç noktalarınızı yanlış.|
|Özür dileriz, CDN yönetim hizmeti şu anda kullanılamıyor | Geri durum güncelleştirmeleri için burayı tıklatın; Beklenen çözümleme süresi sonra sorununuz devam ederse desteğe başvurun.|
|CDN uç noktalarınızı bazı bizim CDN sağlayıcıları ile devam eden sorunları tarafından etkilenebilir ne yazık ki | Geri durum güncelleştirmeleri için burayı tıklatın; Kaynak ve CDN uç noktası test etmek öğrenmek için sorun giderme aracını kullanın; Beklenen çözümleme süresi sonra sorununuz devam ederse desteğe başvurun. |
|CDN uç noktası yapılandırma değişikliklerini yayma gecikmeleri yaşıyor ne yazık ki | Geri durum güncelleştirmeleri için burayı tıklatın; Yapılandırma değişiklikleri beklenen süre içinde tam olarak yayılmaz ederse Destek'e başvurun.|
|Şu ek portal yükleme sorunlar yaşıyoruz ne yazık ki | Geri durum güncelleştirmeleri için burayı tıklatın; Beklenen çözümleme süresi sonra sorununuz devam ederse desteğe başvurun.|
Üzgünüz, şu bizim CDN sağlayıcıları bazı sorunlar yaşıyoruz | Geri durum güncelleştirmeleri için burayı tıklatın; Beklenen çözümleme süresi sonra sorununuz devam ederse desteğe başvurun. |

## <a name="next-steps"></a>Sonraki adımlar

- [Azure kaynak durumu genel bir bakış okuma](../resource-health/resource-health-overview.md)
- [CDN sıkıştırma ile ilgili sorunları giderme](./cdn-troubleshoot-compression.md)
- [404 hataları ile ilgili sorunları giderme](./cdn-troubleshoot-endpoint.md)