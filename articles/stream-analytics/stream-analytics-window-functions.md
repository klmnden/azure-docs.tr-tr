---
title: Azure Stream Analytics Pencereleme işlevleri giriş
description: Bu makalede Azure Stream Analytics işlerine kullanılan dört Pencereleme işlevleri (dönen, atlamalı, kayan, oturum) açıklanmaktadır.
services: stream-analytics
author: jseb225
ms.author: jeanb
manager: kfile
ms.reviewer: jasonh
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 05/07/2018
ms.openlocfilehash: 2650058e277bc0338c779655ce381be046fb120a
ms.sourcegitcommit: 870d372785ffa8ca46346f4dfe215f245931dae1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/08/2018
---
# <a name="introduction-to-stream-analytics-windowing-functions"></a>Stream Analytics Pencereleme işlevleri giriş
Zaman akış senaryolarda, zamana bağlı Windows'da bulunan veriler üzerinde işlemler genel bir desen olur. Akış analizi, geliştiriciler için en az çaba ile yazar karmaşık akış işleme işleri etkinleştirme Pencereleme işlevler için yerel destek vardır.

Aralarından seçim yapabileceğiniz zamana bağlı windows dört tür vardır: [ **dönen**](https://msdn.microsoft.com/azure/stream-analytics/reference/tumbling-window-azure-stream-analytics), [ **Hopping**](https://msdn.microsoft.com/azure/stream-analytics/reference/hopping-window-azure-stream-analytics), [  **Kayan**](https://msdn.microsoft.com/azure/stream-analytics/reference/sliding-window-azure-stream-analytics), ve [ **oturum** ](https://msdn.microsoft.com/azure/stream-analytics/reference/session-window-azure-stream-analytics) windows.  Pencere işlevlerde kullandığınız [ **GROUP BY** ](https://msdn.microsoft.com/azure/stream-analytics/reference/group-by-azure-stream-analytics) yan tümcesi, akış analizi işleri, sorgu sözdizimi.

Tüm [Pencereleme](https://msdn.microsoft.com/azure/stream-analytics/reference/windowing-azure-stream-analytics) operations çıktı sonuçlarına **son** penceresinin. Çıktı penceresi tek olay kullanılan üzerinde toplama işlevi tabanlı olacaktır. Çıktı olayını penceresinin bitiş zaman damgası sahip olur ve tüm pencere işlevleri sabit uzunluk ile tanımlanır. 

![Stream Analytics penceresi işlevleri kavramları](media/stream-analytics-window-functions/stream-analytics-window-functions-conceptual.png)

## <a name="tumbling-window"></a>Atlayan pencere
Dönen pencere işlevleri ayrı zaman kesimler halinde veri akışı ayırabilir ve bunları karşı işlevi aşağıdaki örnek gibi gerçekleştirmek için kullanılır. Atlayan pencere, anahtar differentiators, bunlar yinelemeniz, olmayan çakışma ve bir olay için birden fazla atlayan pencere ait olamaz ' dir.

![Akış analizi dönen pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-tumbling-intro.png)

## <a name="hopping-window"></a>Atlamalı pencere
Atlamalı pencere işlevleri atlama İleri zamanına göre sabit bir süre içinde. Olayları birden fazla Hopping penceresi sonuç kümesine ait olabilir. böylece bunları binebilir, dönen windows düşünmek kolay olabilir. Atlayan pencere aynı Hopping penceresi oluşturmak için pencere boyutu ile aynı olacak şekilde atlama boyutu belirtin. 

![Stream Analytics atlamalı pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-hopping-intro.png)

## <a name="sliding-window"></a>Kayan pencere
Dönen veya atlamalı windows, farklı olarak, kayan pencere işlevleri bir çıktı üretir **yalnızca** olay oluştuğunda. Her penceresinde en az bir olay sahip olur ve pencere sürekli bir € (epsilon) ileri taşır. Windows atlamalı gibi olaylar için birden fazla kayan pencere ait olabilir.

![Akış analizi kayan pencere](media/stream-analytics-window-functions/stream-analytics-window-functions-sliding-intro.png)

## <a name="session-window-preview"></a>Oturumu penceresi (Önizleme)
Oturum penceresi işlevleri Grup benzer zamanlarda geldiğinde olayları süreler filtreleme söz konusu olduğunda veri yok. Üç ana parametrelere sahip: zaman aşımı, en uzun süre ve bölümleme anahtarı (isteğe bağlı).

![Stream Analytics oturumu penceresi](media/stream-analytics-window-functions/stream-analytics-window-functions-session-intro.png)

İlk olay gerçekleştiğinde bir oturumu penceresi başlar. Son alınan olayından belirtilen süre içinde başka bir olay meydana gelirse, pencerenin yeni olay içerecek şekilde genişletir. Aksi takdirde zaman aşımı süresi içinde hiçbir olay meydana gelirse, penceresi sırasında zaman aşımı kapatılır.

Olaylar belirtilen süre içinde gerçekleşen tutmak, en uzun süre ulaşılana kadar oturumu penceresi genişletme tutmak. Belirtilen en fazla süre ile aynı boyutta olması için en uzun süre denetimi aralıklarını ayarlanır. Örneğin, varsa denetimleri penceresi aşan maksimum en 10 ise süresi durum t = 0, 10, 20, 30, vs.

Bölüm anahtarı sağlandığında, olaylar anahtarı ile birlikte gruplanır ve oturumu penceresi her gruba bağımsız olarak uygulanır. Bu bölümleme, farklı kullanıcılar veya aygıtlar için farklı oturum windows burada gereken durumlarda yararlıdır.


## <a name="next-steps"></a>Sonraki adımlar
* [Azure Stream Analytics'e giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi'ni kullanmaya başlama](stream-analytics-real-time-fraud-detection.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)

