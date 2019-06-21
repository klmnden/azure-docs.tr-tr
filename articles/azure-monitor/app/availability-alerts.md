---
title: Azure Application Insights ile kullanılabilirliği uyarıları Kurulumu | Microsoft Docs
description: Application Insights’ta web testleri ayarlayın. Web sitesi kullanılamaz duruma gelirse veya yavaş yanıt verirse uyarı alın.
services: application-insights
documentationcenter: ''
author: lgayhardt
manager: carmonm
ms.assetid: 46dc13b4-eb2e-4142-a21c-94a156f760ee
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 06/19/2019
ms.reviewer: sdash
ms.author: lagayhar
ms.openlocfilehash: cc022f91d4b4fec42929769df8c979320548a1f9
ms.sourcegitcommit: 82efacfaffbb051ab6dc73d9fe78c74f96f549c2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67304897"
---
# <a name="availability-alerts"></a>Kullanılabilirlik uyarıları

[Azure Application Insights](../../azure-monitor/app/app-insights-overview.md), dünyanın her yerindeki noktalarından uygulamanıza düzenli aralıklarla web istekleri gönderir. Bu, uygulamanızın yanıt vermiyor veya çok yavaş yanıtlarsa uyarabilir.

## <a name="enable-alerts"></a>Uyarıları etkinleştirme

Uyarılar artık otomatik olarak varsayılan olarak etkindir, ancak tam olarak bir uyarı yapılandırmak için önce ilk olarak, kullanılabilirlik testi oluşturmak zorunda.

![Deneyimi oluşturun](./media/availability-alerts/create-test.png)

> [!NOTE]
>  İle [yeni birleştirilmiş uyarılar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), uyarı kuralının önem derecesi ve bildirim tercihleri ile [Eylem grupları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups) **olmalıdır** uyarılar deneyimi yapılandırılmış. Aşağıdaki adımlar olmadan, yalnızca portal bildirim alırsınız.

1. Kullanılabilirlik testi kaydettikten sonra Ayrıntıları sekmesini üç noktaya yaptığınız test tarafından. "Uyarı Düzenle" tıklayın.

   ![Düzen kaydedildikten sonra](./media/availability-alerts/edit-alert.png)

2. İstenen önem derecesini, kural açıklaması ve en önemlisi - bu uyarı kuralı için kullanmak istediğiniz bildirim tercihleri olan eylem grubu ayarlayın.

   ![Düzen kaydedildikten sonra](./media/availability-alerts/set-action-group.png)

### <a name="alert-on-x-out-of-y-locations-reporting-failures"></a>Hata Raporlama Y konumları dışında X uyar

Varsayılan olarak etkin uyarı kuralı Y konumları dışında X [birleştirilmiş yeni uyarılar deneyimini](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), yeni bir kullanılabilirlik testi oluşturun. "Klasik" seçeneğini belirleyerek veya uyarı kuralı devre dışı bırakmak seçerek geri çevirebilirsiniz.

> [!NOTE]
> Yukarıdaki adımları izleyerek uyarıyı tetikleyecek çıktığında bildirimleri almak için Eylem grupları yapılandırın. Kural tetiklendiğinde bu adım, yalnızca portal bildirim alırsınız.
>

### <a name="alert-on-availability-metrics"></a>Kullanılabilirlik ölçümler üzerinde uyarı

Kullanarak [yeni birleştirilmiş uyarılar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), uyarı segmentli toplama kullanılabilirliğine ve süresi ölçümleri test edebilirsiniz:

1. Ölçüm deneyimi bir Application Insights kaynağını seçin ve bir kullanılabilirlik ölçümü seçin:

    ![Kullanılabilirlik ölçümlerini seçimi](./media/availability-alerts/select-metric.png)

2. Menü seçeneğinden belirli testleri veya uyarı kuralı ayarlamak için konumları seçebileceğiniz yeni deneyime sürer uyarıları yapılandırın. Bu uyarı kuralı buraya Eylem grupları da yapılandırabilirsiniz.

### <a name="alert-on-custom-analytics-queries"></a>Özel analytics sorgularına göre uyarı

Kullanarak [yeni birleştirilmiş uyarılar](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-unified-alerts), sizi uyarabilir [özel günlük sorguları](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitor-alerts-unified-log). Özel sorgular ile kullanılabilirlik sorunları en güvenilir sinyal elde etmenize yardımcı olan rastgele koşula göre uyarabilir. Bu ayrıca TrackAvailability SDK'sını kullanarak özel kullanılabilirlik sonuçları gönderiyorsanız özellikle geçerlidir. 

> [!Tip]
> Ölçümleri kullanılabilirliği verileri TrackAvailability SDK'mız çağırarak gönderdiğiniz herhangi bir özel kullanılabilirlik sonucunu içerir. Uyarı özel kullanılabilirlik sonuçları ölçümleri desteği hakkında uyarı kullanabilirsiniz.
>

## <a name="troubleshooting"></a>Sorun giderme

Ayrılmış [sorunlarını giderme makalesine](troubleshoot-availability.md).

## <a name="next-steps"></a>Sonraki adımlar

* [Çok adımlı web testleri](availability-multistep.md)
* [URL ping web testleri](monitor-web-app-availability.md)

