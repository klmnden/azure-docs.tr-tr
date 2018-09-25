---
title: Azure ön kapısı hizmeti - Ölçümler ve günlüğe kaydetme | Microsoft Docs
description: Bu makalede Azure ön kapısı hizmetinin desteklediği günlüklerine erişme ve farklı ölçümlerin anlamanıza yardımcı olur
services: frontdoor
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/18/2018
ms.author: sharadag
ms.openlocfilehash: 643ae03a9350868b172d99b2af7ce23e34fedf47
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/24/2018
ms.locfileid: "46998007"
---
# <a name="monitoring-metrics-for-front-door"></a>Ön kapısı ölçümlerini izleme

Azure ön kapısı hizmetini kullanarak, kullanım, sağlık ve performans veya, ön kapısı yanı sıra ön kapısı yapılandırmasını doğrulamak için önemli ölçümlerinizi izleyebilirsiniz.

## <a name="metrics"></a>Ölçümler

Ölçümler, performans sayaçları portalda görüntüleyebileceğiniz belirli Azure kaynakları için bir özelliğidir. Aşağıdaki ölçümler için ön kapı, kullanılabilir:

| Ölçüm | Ölçüm görünen adı | Birim | Boyutlar | Açıklama |
| --- | --- | --- | --- | --- |
| RequestCount | İstek sayısı | Sayı | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | Ön kapısı tarafından sunulan istemci isteklerinin sayısı.  |
| RequestSize | İstek boyutu | Bayt | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstekleri olarak ön kapısı istemcilerden gönderilen bayt sayısı. |
| ResponseSize | Yanıt boyutu | Bayt | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemcilere ön kapısı yanıt gönderilen bayt sayısı. |
| TotalLatency | Toplam gecikme süresi | Milisaniye | Httpstatus'a</br>HttpStatusGroup</br>ClientRegion</br>ClientCountry | İstemci ön kapısı son yanıt bayttan onaylanır kadar ön kapısı tarafından istemci isteği alındı, hesaplanan saat. |
| BackendRequestCount | Arka uç istek sayısı | Sayı | Httpstatus'a</br>HttpStatusGroup</br>Arka uç | Arka uçları için ön kapı gönderilen isteklerin sayısı. |
| BackendRequestLatency | Arka uç istek gecikme süresi | Milisaniye | Arka uç | Zaman zaman ön kapısı son yanıt bayt arka uçtan alınan kadar isteği arka uca ön kapısı tarafından gönderildiği hesaplanır. |
| BackendHealthPercentage | Arka uç sistem durumu yüzdesi | Yüzde | Arka uç</br>BackendPool | Başarılı sistem durumu yüzdesi arka uçları için ön kapı araştırmaları. |
| WebApplicationFirewallRequestCount | Web uygulaması güvenlik duvarı istek sayısı | Sayı | PolicyName</br>RuleName</br>Eylem | Ön kapısı uygulama katman güvenlik tarafından işlenen istemci isteklerinin sayısı. |


## <a name="next-steps"></a>Sonraki adımlar

- Bilgi edinmek için nasıl [ön kapı oluşturmak](quickstart-create-front-door.md).
- Bilgi [ön kapısı işleyişi](front-door-routing-architecture.md).
