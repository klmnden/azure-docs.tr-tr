---
title: Uzaktan izleme çözümünde - Azure izleme Gelişmiş | Microsoft Docs
description: Bu öğretici, cihazları Uzaktan izleme çözüm panosunu ile izleme gösterir.
services: iot-suite
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 02/22/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: 0456594a4a7776175781968779b4540a98070b78
ms.sourcegitcommit: b6319f1a87d9316122f96769aab0d92b46a6879a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/20/2018
---
# <a name="perform-advanced-monitoring-using-the-remote-monitoring-solution"></a>Gelişmiş Uzaktan izleme çözümü kullanarak izleme gerçekleştirme

Bu öğretici Uzaktan izleme Panosu özelliklerini gösterir. Bu özellikler tanıtmak için öğretici senaryo Contoso IOT uygulamada kullanır.

Bu öğreticide, aygıtlarınızı Çözüm Hızlandırıcısı panosundan izlemek öğrenmek için iki sanal Contoso kamyon cihazlar kullanın. Contoso operatör olarak, konum ve alanında, kamyonlar davranışını izlemeniz gerekir.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

>[!div class="checklist"]
> * Aygıtları panosunda filtreleme
> * Görünüm gerçek zamanlı telemetri
> * Cihaz ayrıntıları görüntüle
> * Cihazlarınızdan gelen Uyarıları görüntüle
> * Sistem KPI'ları görüntüleyin

## <a name="prerequisites"></a>Önkoşullar

Bu öğreticiyi izlemek için Azure aboneliğinizde Uzaktan izleme çözümü dağıtılan bir örneğini gerekir.

Uzaktan izleme çözümü dağıtılan henüz henüz tamamlanmış olmalıdır, [Uzaktan izleme Çözüm Hızlandırıcısı dağıtmak](iot-accelerators-remote-monitoring-deploy.md) Öğreticisi.

## <a name="choose-the-devices-to-display"></a>Cihazları görüntülemek için seçin

Hangi cihazların görüntülemek seçmek için **Pano** sayfasında, filtreleri kullanın. Yalnızca görüntülemek için **kamyon** aygıtları seçin yerleşik **kamyonlar** Filtre açılan Filtresi:

![Panoda kamyonlar Filtrele](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter.png)

Bir filtre uyguladığınızda, filtre koşulları eşleşen aygıtlar üzerinde eşlemesinde görüntüler **Pano** sayfa:

![Harita üzerinde kamyonlar görüntüleme](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap.png)

Filtre de içinde gördüğünüz hangi aygıtların belirler **Telemetri** grafiği:

![Panoda kamyon telemetri görüntüler](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry.png)

Oluşturmak için düzenlemek ve filtreleri silmek, seçin **yönetmek filtreleri**.

## <a name="view-real-time-telemetry"></a>Görünüm gerçek zamanlı telemetri

Çözüm Hızlandırıcısı üzerinde ayrıntılı gerçek zamanlı telemetri verileri grafiğe çizer **Pano** sayfası. Telemetri grafik geçerli filtre tarafından seçili cihazlar için telemetri bilgileri gösterir:

![Kamyon telemetri çizim](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview.png)

Görüntülemek için telemetri değerlerini seçmek için grafiğin üstünde telemetri türünü seçin:

![Kamyon telemetri çizim](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry.png)

<!-- 05/01 - this features appears to have been removed
To pause the live telemetry display, choose **Flowing**. To re-enable the live display, choose **Pause**:

![Pause and restart telemetry display](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetrypause.png)-->

## <a name="use-the-map"></a>Harita kullanın

Harita geçerli filtre tarafından seçilen benzetimli kamyonlar hakkındaki bilgileri görüntüler. Yakınlaştırma ve eşleme konumları fazla veya az ayrıntılı olarak görüntülemek için kaydır. Harita üzerinde aygıt simgelerini herhangi belirtmek **uyarıları** veya **uyarıları** aygıt için etkin olan. Sayısı özetini **uyarıları** ve **uyarıları** harita solunda görüntüler.

<!-- 05/01 - cannot select a deice on the map
To view the device details, pan and zoom the map to locate the devices, then click the device on the map. The details include:

* Recent telemetry values
* Methods the device supports
* Device properties

![View device details on the dashboard](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail.png)-->

## <a name="view-alerts-from-your-devices"></a>Cihazlarınızdan gelen Uyarıları görüntüle

Geçerli filtre ile aygıtları harita vurgular **uyarıları** ve **uyarıları**. **Uyarıları** Masası cihazlarınızdan en son uyarıların ilgili ayrıntılı bilgileri görüntüler:

![Panoda sistem uyarıları görüntüle](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms.png)

Kullanabileceğiniz **Pano** son uyarıları için zaman aralığını ayarlamak için filtre. Varsayılan olarak, son bir saat uyarılardan panelinde görüntülenir:

![Uyarıları zamanına göre filtrele](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter.png)

## <a name="view-the-system-kpis"></a>Sistem KPI'ları görüntüleyin

**Pano** sayfa sistem KPI'ları görüntüler:

![Pano KPI'ları](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis.png)

Kullanabileceğiniz **Pano** KPI toplama zaman aralığı ayarlamak için filtre. Varsayılan olarak, son saatten fazla toplanan KPI'ları panelinde görüntülenir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğretici nasıl kullanılacağı gösterilmiştir **Pano** filtre ve Uzaktan izleme çözümünde sağlanan benzetimli kamyonlar izlemek için sayfa:

<!-- Repeat task list from intro -->
>[!div class="checklist"]
> * Aygıtları panosunda filtreleme
> * Görünüm gerçek zamanlı telemetri
> * Cihaz ayrıntıları görüntüle
> * Cihazlarınızdan gelen Uyarıları görüntüle
> * Sistem KPI'ları görüntüleyin

Aygıtlarınızı izlemek öğrendiniz, bilgi edinmek için önerilen sonraki adımlar olan nasıl yapılır:

* [Eşik tabanlı kurallar kullanarak sorunlarını algılamak](iot-accelerators-remote-monitoring-automate.md).
* [Yönetmek ve cihazlarınızı yapılandırmak](iot-accelerators-remote-monitoring-manage.md).
* [Aygıt sorunlarını gidermek ve](iot-accelerators-remote-monitoring-maintain.md).
* [Sanal cihazlar ile çözümünüzü test](iot-accelerators-remote-monitoring-test.md).

<!-- Next tutorials in the sequence -->