---
title: Azure CDN içindeki gerçek zamanlı İstatistikler | Microsoft Docs
description: Gerçek zamanlı İstatistikler istemcilerinize içerik teslim edilirken Azure CDN performansıyla ilgili gerçek zamanlı verileri sağlar.
services: cdn
documentationcenter: ''
author: zhangmanling
manager: erikre
editor: ''
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: e9b9522de6b2c54dc794b00100ffe358296ecfdd
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/11/2017
ms.locfileid: "23842884"
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>Microsoft Azure cdn'de gerçek zamanlı İstatistikler
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış
Bu belgede, Microsoft Azure cdn'de gerçek zamanlı İstatistikler açıklanmaktadır.  Bu işlevsellik, istemcilerinize içerik teslim edilirken bant genişliği, önbellek durumları ve CDN profilinizi yapılan eşzamanlı bağlantı gibi gerçek zamanlı verileri sağlar. Bu, sürekli Hizmetinizin durumunu servise olaylar dahil olmak üzere, herhangi bir zamanda izleme sağlar.

Aşağıdaki grafikleri kullanılabilir:

* [Bant genişliği](#bandwidth)
* [Durum kodları](#status-codes)
* [Önbellek durumları](#cache-statuses)
* [Bağlantılar](#connections)

## <a name="accessing-real-time-stats"></a>Gerçek zamanlı İstatistikler erişme
1. İçinde [Azure Portal](https://portal.azure.com), CDN profilinize gidin.
   
    ![CDN profili dikey penceresi](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. CDN profili dikey penceresinden tıklayın **Yönet** düğmesi.
   
    ![CDN profili dikey penceresi yönetmek düğmesi](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    CDN Yönetim Portalı'nı açar.
3. Üzerine gelerek **Analytics** sekmesini ve ardından üzerine gelerek **gerçek zamanlı İstatistikler** çıkma.  Tıklayın **HTTP büyük nesne**.
   
    ![CDN Yönetim Portalı](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    Gerçek zamanlı İstatistikler grafikleri görüntülenir.

Her grafikleri sayfa yüklendiğinde başlangıç seçilen zaman aralığı için gerçek zamanlı istatistikler görüntüler.  Grafikler, her birkaç saniyede otomatik olarak güncelleştirilir.  **Grafiği Yenile** , varsa, Temizle düğmesi daha sonra yalnızca gösterilir ve seçili verilerin grafik.

## <a name="bandwidth"></a>Bant Genişliği
![Bant genişliği grafiği](./media/cdn-real-time-stats/cdn-bandwidth.png)

**Bant genişliği** grafiği geçerli platform için seçilen zaman aralığı içinde kullanılan bant genişliği miktarını görüntüler. Grafik gölgeli kısmı bant genişliği kullanımını gösterir. Tam şu anda kullanılan bant genişliği miktarını doğrudan çizgi grafiği altında görüntülenir.

## <a name="status-codes"></a>Durum kodları
![Durum kodu grafiği](./media/cdn-real-time-stats/cdn-status-codes.png)

**Durum kodları** grafik belirli HTTP yanıt kodları seçilen zaman aralığı içinde ne sıklıkta gerçekleştiğini gösterir.

> [!TIP]
> HTTP durum kodu seçeneğin açıklaması için bkz: [Azure CDN HTTP durum kodları](https://msdn.microsoft.com/library/mt759238.aspx).
> 
> 

HTTP durum kodları listesini doğrudan grafik görüntülenir. Bu liste çizgi grafiği ve bu durum kodu için saniye başına yinelenme sayısı dahil her durum kodu gösterir. Varsayılan olarak, bu durum kodunun grafikteki her biri için bir satır görüntülenir. Ancak, yalnızca CDN yapılandırmanız için özel öneme sahip durum kodları izlemek için seçebilirsiniz. Bunu yapmak için istenen durum kodları denetleyin ve diğer tüm seçenekleri temizleyin ve ardından **Grafiği Yenile**. 

Bir özel durum kodu için günlüğe kaydedilen verileri geçici olarak gizleyebilirsiniz.  Grafiğin altındaki doğrudan göstergeden gizlemek istediğiniz durum kodu'ı tıklatın. Durum kodu hemen grafikten gizlenir. Bu durum kodu yeniden tıklandığında yeniden görüntülenmesi için bu seçeneği neden olur.

## <a name="cache-statuses"></a>Önbellek durumları
![Önbellek durumları grafiği](./media/cdn-real-time-stats/cdn-cache-status.png)

**Önbellek durumları** grafik belirli türde bir önbellek durumları seçilen zaman aralığı içinde ne sıklıkta gerçekleştiğini gösterir. 

> [!TIP]
> Her önbellek durum kodu seçeneği açıklaması için bkz: [Azure CDN önbellek durum kodları](https://msdn.microsoft.com/library/mt759237.aspx).
> 
> 

Önbellek durum kodları listesini doğrudan grafik görüntülenir. Bu liste çizgi grafiği ve bu durum kodu için saniye başına yinelenme sayısı dahil her durum kodu gösterir. Varsayılan olarak, bu durum kodunun grafikteki her biri için bir satır görüntülenir. Ancak, yalnızca CDN yapılandırmanız için özel öneme sahip durum kodları izlemek için seçebilirsiniz. Bunu yapmak için istenen durum kodları denetleyin ve diğer tüm seçenekleri temizleyin ve ardından **Grafiği Yenile**. 

Bir özel durum kodu için günlüğe kaydedilen verileri geçici olarak gizleyebilirsiniz.  Grafiğin altındaki doğrudan göstergeden gizlemek istediğiniz durum kodu'ı tıklatın. Durum kodu hemen grafikten gizlenir. Bu durum kodu yeniden tıklandığında yeniden görüntülenmesi için bu seçeneği neden olur.

## <a name="connections"></a>Bağlantılar
![Bağlantıları grafiği](./media/cdn-real-time-stats/cdn-connections.png)

Bu grafik, kaç tane bağlantıları kenar sunucularınızı kurulmuş gösterir. Her istek için bir bağlantı bizim CDN sonuçlarında geçtiği bir varlık.

## <a name="next-steps"></a>Sonraki Adımlar
* İle bilgi edinin [Azure CDN gerçek zamanlı uyarılar](cdn-real-time-alerts.md)
* Dıg ile daha derin [Gelişmiş HTTP raporları](cdn-advanced-http-reports.md)
* Analiz [kullanım desenleri](cdn-analyze-usage-patterns.md)

