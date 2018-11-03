---
title: Azure Log Analytics'te özel bir pano oluşturun | Microsoft Docs
description: Bu kılavuz, Log Analytics panoları tüm kayıtlı günlük aramalarınızı nasıl görselleştirebilirsiniz anlamanıza ortamınızı görüntülemek için tek bir mercek sunarak yardımcı olur.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/08/2017
ms.author: magoedte
ms.component: ''
ms.openlocfilehash: d906515214e042a09d434f02be1778c275f214a8
ms.sourcegitcommit: ada7419db9d03de550fbadf2f2bb2670c95cdb21
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/02/2018
ms.locfileid: "50958143"
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Log Analytics'te kullanım için özel bir pano oluşturma

Bu kılavuz, Log Analytics panoları tüm kayıtlı günlük aramalarınızı nasıl görselleştirebilirsiniz anlamanıza ortamınızı görüntülemek için tek bir mercek sunarak yardımcı olur.

>[!NOTE]
> Artık mevcut düzenleyebilirsiniz **Panom'u**. Bu özellik, kullanım dışı aşamasında kullanılabilir.

![Örnek Pano](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

OMS portalında oluşturduğunuz özel panolar da OMS mobil uygulamada kullanılabilir. Uygulamalar hakkında daha fazla bilgi için aşağıdaki sayfalara bakın.

* [OMS mobil uygulamasını Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [OMS mobil uygulamasını Apple iTunes](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![Mobil Pano](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Benim Panom nasıl oluşturulur?
Başlamak için OMS Genel Bakış sayfasına gidin. Göreceğiniz **Panom'u** kutucuğuna soldaki. Panonuzu detaya gitmek için tıklayın.

![Genel Bakış](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Bir kutucuk ekleme
Panoları, kutucukları kayıtlı günlük aramalarınızı tarafından desteklenir. OMS hemen başlayabileceğiniz kayıtlı günlük aramalarınızı önceden yapılan çoğu ile birlikte gelir. Başlamak nasıl anahat aşağıdaki adımları kullanın.

Panom'u Görünümü'nde tıklamanız yeterlidir **Özelleştir** girmek için modu özelleştirin.

![Resimsel](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Sayfanın sağ tarafta açılır paneli, tüm kayıtlı günlük aramalarınızı çalışma alanınızın gösterir. Bir kutucuk olarak kaydedilmiş günlük aramasını görselleştirme için kayıtlı bir aramayı gelin ve ardından **artı** simgesi.

![Kutucuk 1 Ekle](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Tıkladığınızda **artı** sembol, My Pano Görünümü'nde yeni bir kutucuk görünür.

![2 kutucuk Ekle](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Kutucukları düzenleme
Panom'u Görünümü'nde tıklamanız yeterlidir **Özelleştir** girmek için modu özelleştirin. Düzenlemek istediğiniz kutucuğa tıklayın. Düzenlemek için sağ panelde değişiklikleri ve seçim seçenekleri sağlar:

![Kutucukları düzenleme](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Kutucukları düzenleme](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Döşeme görselleştirmeleri
Döşeme görselleştirmeleri aralarından seçim yapabileceğiniz üç tür vardır:

| grafik türü | Ne işe yarar |
| --- | --- |
| ![Çubuk grafik](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Günlük arama veya bir alana göre sonuçları toplar, kaydedilmiş log search'ün sonuçları belirli bir zaman çizelgesi çubuk grafik veya bağlı olarak bir alana göre sonuçları listesi olarak görüntüler. |
| ![ölçüm](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Toplam günlük arama sonucu isabet sayısı bir döşeme içindeki bir sayı olarak görüntüler. Ölçüm kutucukları Eşiğe ulaşıldığında, kutucuğu vurgular bir eşiği ayarlamanıza olanak sağlar. |
| ![satır](./media/log-analytics-dashboards/oms-dashboards-line.png) |Bir zaman çizelgesi, kayıtlı günlük arama sonucu isabet değerleriyle bir çizgi grafik görüntüler. |

### <a name="threshold"></a>Eşik
Ölçüm görselleştirmeyi kullanarak kutucuk üzerinde bir eşiği oluşturabilirsiniz. Kutucuğa bir eşik değeri oluşturulacağını seçin. Değerin üzerinde veya seçilen eşiğin altında ve ardından eşik değerini ayarlayın kutucuğu vurgulamak bu seçeneği seçin.

## <a name="organizing-the-dashboard"></a>Pano düzenleme
Panonuzda düzenlemek için Panom'u görünümüne gidin ve **Özelleştir** girmek için modu özelleştirin. Taşıma ve taşımak kutucuğunuzu olmasını istediğiniz istediğiniz kutucuğu sürükleyip bırakın.

![Pano düzenleme](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Bir kutucuğu Kaldır
Bir kutucuğu kaldırmak için Panom'u görünümüne gidin ve **Özelleştir** girmek için modu özelleştirin. Kaldırın ve ardından Sağdaki panelde istediğiniz kutucuğu seçin **Kaldır kutucuğu**.

![Bir kutucuğu Kaldır](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturma [uyarılar](../monitoring-and-diagnostics/monitoring-overview-unified-alerts.md) Log analytics'te bildirimleri oluşturmak için ve sorunları düzeltin.
