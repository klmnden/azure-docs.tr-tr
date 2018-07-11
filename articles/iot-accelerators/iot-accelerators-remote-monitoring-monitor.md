---
title: IoT cihazlarınızı bir Azure çözümünden izleme | Microsoft Docs
description: Bu öğreticide Uzaktan İzleme çözümü hızlandırıcısını kullanarak IoT cihazlarınızı izlemeyi öğreneceksiniz.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 06/08/2018
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 5f42ed0fa5362959e5619f2d550ca1ae3711ed65
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/29/2018
ms.locfileid: "37097470"
---
# <a name="tutorial-monitor-your-iot-devices"></a>Öğretici: IoT cihazlarınızı izleme

Bu öğreticide bağlı IoT cihazlarınızı izlemek için Uzaktan İzleme çözümü hızlandırıcısını kullanacaksınız. Çözüm panosunu kullanarak telemetri verilerini, cihaz bilgilerini, uyarıları ve KPI'leri görüntüleyeceksiniz.

Öğreticide bu izleme özelliklerini kullanmak için iki sanal tır cihazı kullanılmaktadır. Tırlar, Contoso adlı bir kuruluş tarafından yönetilmektedir ve Uzaktan İzleme çözümü hızlandırıcısına bağlıdır. Contoso'da çalışan bir operatör olarak sahadaki tırların konumunu ve davranışını izlemeniz gerekmektedir.

Bu öğreticide şunları yaptınız:

>[!div class="checklist"]
> * Panodaki cihazları filtreleme
> * Gerçek zamanlı telemetri verilerini görüntüleme
> * Cihaz ayrıntılarını görüntüleme
> * Cihazlarınızdan gelen uyarıları görüntüleme
> * Sistem KPI'lerini görüntüleme

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi takip etmek için Azure aboneliğinizde Uzaktan İzleme çözümü hızlandırıcısının dağıtılmış örneğine sahip olmanız gerekir.

Uzaktan İzleme çözümü hızlandırıcısını henüz dağıtmadıysanız [Bulut tabanlı uzaktan izleme çözümü dağıtma](quickstart-remote-monitoring-deploy.md) hızlı başlangıcını tamamlamanız gerekir.

## <a name="choose-the-devices-to-display"></a>Görüntülenecek cihazları seçme

**Dashboard** (Pano) sayfasında görüntülenecek bağlı cihazları seçmek için filtreleri kullanın. Yalnızca **Truck** (Tır) cihazlarını görüntülemek için filtre açılan menüsünden yerleşik **Trucks** (Tırlar) filtresini seçin:

[![Panoda trucks (tır) filtresi](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckfilter-expanded.png#lightbox)

Bir filtre uyguladığınızda **Dashboard** (Pano) sayfasındaki haritada yalnızca filtre koşullarıyla eşleşen cihazlar görüntülenir:

[![Haritada yalnızca tırlar görüntülenir](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtruckmap-expanded.png#lightbox)

Filtre aynı zamanda **Telemetry** (Telemetri) grafiğinde göreceğiniz cihazları da belirler:

[![Panoda tırların telemetri verileri görüntülenir](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetry-expanded.png#lightbox)

Filtre oluşturmak, düzenlemek ve silmek için **Manage device groups** (Cihaz gruplarını yönet) öğesini seçin.

## <a name="view-real-time-telemetry"></a>Gerçek zamanlı telemetri verilerini görüntüleme

Çözüm hızlandırıcısı, **Dashboard** (Pano) sayfasındaki grafiğe gerçek zamanlı telemetri verilerini çizer. Telemetri grafiğinin en üstünde geçerli filtre ile seçilen cihazlar için kullanılabilen telemetri türleri gösterilir:

[![Tır telemetri türleri](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardtelemetryview-expanded.png#lightbox)

Sıcaklık telemetrisini görüntülemek için **Temperature** (Sıcaklık) öğesine tıklayın:

[![Tır sıcaklık telemetrisi çizimi](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardselecttelemetry-expanded.png#lightbox)

## <a name="use-the-map"></a>Haritayı kullanma

Haritada geçerli filtre ile seçilmiş olan sanal tırlar hakkındaki bilgiler görüntülenir. Konum ayrıntılarını azaltmak veya artırmak için haritada yakınlaştırma ve kaydırma yapabilirsiniz. Harita üzerindeki cihaz simgesinin rengi **Alerts** (Uyarılar) veya **Warnings** (Uyarılar) olduğunu gösterecek şekilde değişir. Haritanın sol tarafında **Alerts** (Uyarılar) ve **Warnings** (Uyarılar) için özet sayı görüntülenir.

Cihaz ayrıntılarını görüntülemek için haritada yakınlaştırma veya kaydırma yaparak cihazı bulun ve haritadan cihazı seçin. Ardından cihaz etiketini seçerek **Device details** (Cihaz ayrıntıları) panelini açın. Cihaz ayrıntıları panelinde şu veriler bulunur:

* Son telemetri değerleri
* Cihazın desteklediği metotlar
* Cihaz özellikleri

[![Panoda cihaz ayrıntılarını görüntüleme](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboarddevicedetail-expanded.png#lightbox)

## <a name="view-alerts"></a>Uyarıları görüntüleme

**Alerts** (Uyarılar) paneli, cihazlarınızdan gelen son uyarılarla ilgili ayrıntılı bilgileri görüntüler:

[![Panoda cihaz uyarılarını görüntüleme](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardsystemalarms-expanded.png#lightbox)

Son uyarıların süresini ayarlamak için bir filtre kullanabilirsiniz. Panel varsayılan olarak son bir saat içindeki uyarıları görüntüler:

[![Uyarıları zamana göre filtreleme](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardalarmsfilter-expanded.png#lightbox)

## <a name="view-the-system-kpis"></a>Sistem KPI'lerini görüntüleme

**Dashboard** (Pano) sayfasında **Analytics** (Analiz) panelindeki çözüm hızlandırıcısı tarafından hesaplanan sistem KPI'leri görüntülenir:

[![Pano KPI'leri](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/dashboardkpis-expanded.png#lightbox)

Panoda seçilen uyarılar için geçerli cihaza ve zaman filtrelerine göre belirlenmiş üç KPI gösterilir:

* En fazla uyarıyı tetikleyen kurallar için etkin uyarı sayısı.
* Uyarıların cihaz türüne oranı.
* Kritik uyarıların yüzdesi.

KPI'lerin nasıl toplanacağını uyarılar için süreyi ayarlayan ve görüntülenen cihazları denetleyen filtreler belirler. Panelde varsayılan olarak son bir saat için toplu KPI'ler görüntülenir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bir sonraki öğreticiye geçmeyi planlıyorsanız Uzaktan İzleme çözümü hızlandırıcısı dağıtımını bırakın. Kullanmadığınız zaman çözüm hızlandırıcısının maliyetlerini düşürmek için ayarlar panelinden sanal cihazları durdurabilirsiniz:

[![Telemetriyi duraklatma](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-inline.png)](./media/iot-accelerators-remote-monitoring-monitor/togglesimulation-expanded.png#lightbox)

Bir sonraki öğreticiye başlamaya hazır olduğunuzda sanal cihazları yeniden başlatabilirsiniz.

Çözüm hızlandırıcısına ihtiyacınız kalmadıysa [Provisioned solutions](https://www.azureiotsolutions.com/Accelerators#dashboard) (Sağlanan çözümler) sayfasından silebilirsiniz:

![Çözümü sil](media/iot-accelerators-remote-monitoring-monitor/deletesolution.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide sanal tırları filtrelemek ve izlemek amacıyla Uzaktan İzleme çözümü hızlandırıcısındaki **Dashboard** (Pano) sayfasını nasıl kullanacağınız gösterilmiştir. Çözüm hızlandırıcısını bağlı cihazlarınızdaki sorunları algılama amacıyla kullanmayı öğrenmek için bir sonraki öğreticiye geçin.

> [!div class="nextstepaction"]
> [İzleme çözümünüze bağlı cihazlarla sorunları algılama](iot-accelerators-remote-monitoring-automate.md)