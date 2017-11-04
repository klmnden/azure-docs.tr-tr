---
title: "Azure günlük analizi özel bir pano oluşturun | Microsoft Docs"
description: "Bu kılavuz, günlük analizi panolar tüm kaydedilmiş günlük işlemlerinizin nasıl görselleştirebilirsiniz anlamanıza ortamınızı görüntülemek için tek bir mercek vermiş yardımcı olur."
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/08/2017
ms.author: magoedte
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 22cc516c15353e39c73e762d2b8fa0d787a05ef4
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/03/2017
---
# <a name="create-a-custom-dashboard-for-use-in-log-analytics"></a>Günlük analizi kullanmak için özel bir pano oluşturun

Bu kılavuz, günlük analizi panolar tüm kaydedilmiş günlük işlemlerinizin nasıl görselleştirebilirsiniz anlamanıza ortamınızı görüntülemek için tek bir mercek vermiş yardımcı olur.

>[!NOTE]
> Çalışma alanınız için yükseltildiyse [yeni günlük analizi sorgu dili](log-analytics-log-search-upgrade.md), var olan düzenleyemezsiniz sonra **My Pano**. Çalışma alanınızı herhangi değilse **My Pano** eklenen görmezsiniz sonra döşeme **My Pano** yükseltilmiş alanınızdaki. 

![Örnek Pano](./media/log-analytics-dashboards/oms-dashboards-example-dash.png)

OMS portalında oluşturduğunuz özel panolar de OMS mobil uygulamada kullanılabilir. Aşağıdaki sayfalarda uygulamalar hakkında daha fazla bilgi için bkz.

* [OMS mobil uygulamanızdan Microsoft Store](http://www.windowsphone.com/store/app/operational-insights/4823b935-83ce-466c-82bb-bd0a3f58d865)
* [Apple iTunes mobil uygulamadan OMS](https://itunes.apple.com/app/microsoft-operations-management/id1042424859?mt=8)

![Mobil Pano](./media/log-analytics-dashboards/oms-search-mobile.png)

## <a name="how-do-i-create-my-dashboard"></a>Benim Panom nasıl oluşturulur?
Başlamak için OMS Genel Bakış sayfasına gidin. Göreceğiniz **My Pano** döşeme sol tarafta. Panonuza detaya gitmek için tıklatın.

![Genel Bakış](./media/log-analytics-dashboards/oms-dashboards-overview.png)

## <a name="adding-a-tile"></a>Bir kutucuk ekleme
Uygulamasındaki panolar, kutucuk tarafından kaydedilmiş günlük işlemlerinizin güç sağlar. OMS hemen başlayabilmesi için kaydedilmiş günlük aramalar, önceden yapılan çoğu ile birlikte gelir. Başlamak nasıl anahat aşağıdaki adımları kullanın.

My Pano görünümünde tıklamanız yeterlidir **Özelleştir** girmek için özelleştirme moduna.

![Resimsel](./media/log-analytics-dashboards/oms-dashboards-pictorial01.png)

 Sayfanın sağ tarafında açılır paneli tüm çalışma alanınızı 's kaydedilmiş günlük aramaları gösterir. Bir kutucuk kaydedilmiş günlük arama görselleştirmek için kaydedilmiş bir aramayı üzerine gelin ve ardından **artı** simgesi.

![Kutucuk 1 Ekle](./media/log-analytics-dashboards/oms-dashboards-pictorial02.png)

Tıkladığınızda **artı** simge, yeni bir kutucuk My Pano görünümünde görüntülenir.

![Kutucuk 2 Ekle](./media/log-analytics-dashboards/oms-dashboards-pictorial03.png)

## <a name="edit-a-tile"></a>Bir kutucuk Düzenle
My Pano görünümünde tıklamanız yeterlidir **Özelleştir** girmek için özelleştirme moduna. Düzenlemek istediğiniz kutucuğa tıklayın. Düzenlemek için sağ panelde değişiklikleri ve Seçenekler seçim verir:

![Döşeme Düzenle](./media/log-analytics-dashboards/oms-dashboards-pictorial04.png)

![Döşeme Düzenle](./media/log-analytics-dashboards/oms-dashboards-pictorial05.png)

### <a name="tile-visualizations"></a>Döşeme görselleştirmeleri
Aralarından seçim yapabileceğiniz döşeme görselleştirmeleri üç tür vardır:

| grafik türü | ne yapar |
| --- | --- |
| ![Çubuk grafiği](./media/log-analytics-dashboards/oms-dashboards-bar-chart.png) |Günlük arama sonuçları bir alana göre veya toplar, kaydedilmiş günlük aramanın sonuçları bir zaman çizelgesi olarak bir çubuk grafik veya bağlı olarak bir alana göre sonuçlarının bir listesini görüntüler. |
| ![Ölçüm](./media/log-analytics-dashboards/oms-dashboards-metric.png) |Toplam günlük arama sonucu isabet döşeme içinde bir sayı olarak görüntüler. Ölçüm döşeme Eşiğe ulaşıldığında döşeme vurgular bir eşiği ayarlamanıza olanak sağlar. |
| ![Satır](./media/log-analytics-dashboards/oms-dashboards-line.png) |Bir zaman çizelgesi, kaydedilmiş günlük arama sonucu isabet değerleri içeren bir çizgi grafiği görüntüler. |

### <a name="threshold"></a>Eşik
Ölçüm görselleştirme kullanarak bir kutucuğu bir eşik oluşturabilirsiniz. Bir eşik değeri kutucuğu oluşturmak için seçin. Değerin üzerinde veya seçilen eşiğin altında sonra eşik değerini ayarlayın döşeme vurgulamak seçin.

## <a name="organizing-the-dashboard"></a>Pano düzenleme
Panonuz düzenlemek için My Pano görünümüne gidin ve **Özelleştir** girmek için özelleştirme moduna. Taşıma ve taşıyın kutucuğunuzun olmasını istediğiniz istediğiniz kutucuğu sürükleyip bırakın.

![Pano düzenleme](./media/log-analytics-dashboards/oms-dashboards-organize.png)

## <a name="remove-a-tile"></a>Bir kutucuğunu kaldırma
Bir kutucuk kaldırmak için My Pano görünümüne gidin ve **Özelleştir** girmek için özelleştirme moduna. Kaldırın ve sonra Sağdaki panelde seçmek istediğiniz kutucuğu seçin **kaldırmak döşeme**.

![Bir kutucuğunu kaldırma](./media/log-analytics-dashboards/oms-dashboards-remove-tile.png)

## <a name="next-steps"></a>Sonraki adımlar
* Oluşturma [uyarıları](log-analytics-alerts.md) bildirim oluşturmak ve sorunları düzeltmek için günlük analizi içinde.
