---
title: "Uzaktan izleme çözümünde - Azure izleme Gelişmiş | Microsoft Docs"
description: "Bu öğretici, cihazları Uzaktan izleme çözüm panosunu ile izleme gösterir."
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 11/10/2017
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 3dfc5809344af920540a88e097ce399cce653363
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2017
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Gelişmiş Uzaktan izleme çözümü kullanarak izleme gerçekleştirme

Bu öğretici Uzaktan izleme Panosu özelliklerini gösterir. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Bu öğreticide, önceden yapılandırılmış çözüm panosundan aygıtlarınızı izlemek öğrenmek için iki sanal Contoso kamyon cihazlar kullanın. Contoso operatör olarak, konum ve alanında, kamyonlar davranışını izlemeniz gerekir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Aygıtları panosunda filtreleme
> * Görünüm gerçek zamanlı telemetri
> * Cihaz ayrıntıları görüntüle
> * Görünüm alarmlar cihazlarınızdan
> * Sistem KPI'ları görüntüleyin

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [önceden yapılandırılmış Uzaktan izleme çözümü dağıtma](iot-suite-remote-monitoring-deploy.md) Öğreticisi.

## <a name="choose-the-devices-to-display"></a>Cihazları görüntülemek için seçin

Hangi cihazların görüntülemek seçmek için **Pano** sayfasında, filtreleri kullanın. Yalnızca görüntülemek için **kamyon** aygıtları seçin yerleşik **kamyonlar** Filtre açılan Filtresi:

![Panoda kamyonlar Filtrele](media/iot-suite-remote-monitoring-monitor/dashboardtruckfilter.png)

Bir filtre uyguladığınızda, filtre koşulları eşleşen aygıtlar üzerinde eşlemesinde görüntüler **Pano** sayfa:

![Harita üzerinde kamyonlar görüntüleme](media/iot-suite-remote-monitoring-monitor/dashboardtruckmap.png)

Filtre de içinde gördüğünüz hangi aygıtların belirler **Telemetri** grafiği:

![Panoda kamyon telemetri görüntüler](media/iot-suite-remote-monitoring-monitor/dashboardtelemetry.png)

Oluşturmak için düzenlemek ve filtreleri silmek, seçin **yönetmek filtreleri**.

## <a name="view-real-time-telemetry"></a>Görünüm gerçek zamanlı telemetri

Önceden yapılandırılmış çözüm üzerinde ayrıntılı gerçek zamanlı telemetri verileri grafiğe çizer **Pano** sayfası. Telemetri grafik geçerli filtre tarafından seçili cihazlar için telemetri bilgileri gösterir:

![Kamyon telemetri çizim](media/iot-suite-remote-monitoring-monitor/dashboardtelemetryview.png)

Görüntülemek için telemetri değerlerini seçmek için grafiğin üstünde telemetri türünü seçin:

![Kamyon telemetri çizim](media/iot-suite-remote-monitoring-monitor/dashboardselecttelemetry.png)

Canlı telemetri görüntüleyip duraklatmak için tercih **Flowing**. Dinamik görüntü yeniden etkinleştirmek için tercih **duraklatma**:

![Duraklatma ve telemetri görüntü yeniden başlatın](media/iot-suite-remote-monitoring-monitor/dashboardtelemetrypause.png)

## <a name="use-the-map"></a>Harita kullanın

Harita geçerli filtre tarafından seçilen benzetimli kamyonlar hakkındaki bilgileri görüntüler. Yakınlaştırma ve eşleme konumları fazla veya az ayrıntılı olarak görüntülemek için kaydır. Harita üzerinde aygıt simgelerini herhangi belirtmek **alarmlar** veya **uyarıları** aygıt için etkin olan. Sayısı özetini **alarmlar** ve **uyarıları** harita solunda görüntüler.

Cihaz ayrıntılarını görüntülemek için Kaydır ve cihazları bulmak için harita yakınlaştırma ardından harita aygıtta tıklayın. Ayrıntıları içerir:

* Son telemetri değerleri
* Aygıt yöntemlerini destekler
* Cihaz özellikleri

![Panoda cihaz ayrıntıları görüntüle](media/iot-suite-remote-monitoring-monitor/dashboarddevicedetail.png)

## <a name="view-alarms-from-your-devices"></a>Görünüm alarmlar cihazlarınızdan

Geçerli filtre ile aygıtları harita vurgular **alarmlar** ve **uyarıları**. **Sistem alarmlar** Masası cihazlarınızdan en son alarmlar ilgili ayrıntılı bilgileri görüntüler:

![Panoda görünümünü sistem uyarıları](media/iot-suite-remote-monitoring-monitor/dashboardsystemalarms.png)

Kullanabileceğiniz **sistem alarmlar** yeni uyarılar için zaman aralığını ayarlamak için filtre. Varsayılan olarak, son bir saat gelen uyarılar panelinde görüntülenir:

![Alarmlar zamanına göre filtrele](media/iot-suite-remote-monitoring-monitor/dashboardalarmsfilter.png)

> [!NOTE]
> Zaman Aralık filtresi görmek için sağa kaydırın.

## <a name="view-the-system-kpis"></a>Sistem KPI'ları görüntüleyin

**Pano** sayfa sistem KPI'ları görüntüler:

![Alarmlar zamanına göre filtrele](media/iot-suite-remote-monitoring-monitor/dashboardkpis.png)

Kullanabileceğiniz **sistem KPI** KPI toplama zaman aralığı ayarlamak için filtre. Varsayılan olarak, son saatten fazla toplanan KPI'ları panelinde görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici nasıl kullanılacağı gösterilmiştir **Pano** filtre ve Uzaktan izleme çözümünde sağlanan benzetimli kamyonlar izlemek için sayfa:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Aygıtları panosunda filtreleme
> * Görünüm gerçek zamanlı telemetri
> * Cihaz ayrıntıları görüntüle
> * Görünüm alarmlar cihazlarınızdan
> * Sistem KPI'ları görüntüleyin

Aygıtlarınızı izlemek öğrendiniz, bilgi edinmek için önerilen sonraki adımlar olan nasıl yapılır:

* [Eşik tabanlı kurallar kullanarak sorunlarını algılamak](./iot-suite-remote-monitoring-automate.md).
* [Yönetmek ve cihazlarınızı yapılandırmak](./iot-suite-remote-monitoring-manage.md).
* [Aygıt sorunlarını gidermek ve](./iot-suite-remote-monitoring-maintain.md).
* [Sanal cihazlar ile çözümünüzü test](iot-suite-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->